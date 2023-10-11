---
layout: post
title:  "Slow Zsh on macOS? Have Homebrew installed? Try this"
date:   2023-10-11 01:00:00 -0300
categories: 'work'
---

For the past couple of years since macOS adopted Zsh as its default shell, I've observed a slowdown in the terminal's availability. At first, it was barely noticeable. However, over the past three years, the time it took for a new shell to be available worsened significantly, taking almost a minute to complete.

In my quest to fix the issue mentioned above, I started profiling and debugging what process was impacting starting times of Zsh, in order to see if it was something added by myself, or if it was a process that could be started/loaded asynchronously. Zsh has a built in profiler which can be used by adding `zmodload zsh/zprof` at the beginning and `zprof` at the end of the `~/.zshrc` file. Initially, this indicated `nvm` to be the culprit, which ended up in having to include the following function to load it asynchronously:

```shell
nvm() {
  unset -f nvm
  [ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
  nvm $@ # This copies arguments after nvm
}
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

But even with `nvm` out of the way, the shell still took its sweet time to load and I ended up trying a myriad of other fixes to patch things up and see what was causing this issue. After several hours of googling, I stumbled upon a user called *bmike*, posting on Apple's StackExchange, regarding a similar issue to my own: [https://apple.stackexchange.com/questions/437511/how-do-i-debug-zsh-when-startup-performance-is-slow/437729#437729](https://apple.stackexchange.com/questions/437511/how-do-i-debug-zsh-when-startup-performance-is-slow/437729#437729). Noticing that both the issue and the environment matched, I went ahead and looked into the responses, noticing someone mentioning **Homebrew** as the responsible for this issue. The user noted that the issue is happening even before the contents of `.zshrc` are executed, and realized that **Homebrew** includes the following lines of code in `.zprofile` (or in some cases `.zlogin`):

```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/[your-user-name]/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

For each shell started, the last line will repeat itself (I had almost 3000 lines of evals), ending up in a `.zprofile` file similar to this:

```shell
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/[your-user-name]/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
eval "$(/opt/homebrew/bin/brew shellenv)"
eval "$(/opt/homebrew/bin/brew shellenv)"
eval "$(/opt/homebrew/bin/brew shellenv)"
...
```

Cleaning up the file, leaving only a single evaluation and commenting out the first line, solves the issue. Your resulting file should look something like this:

```shell
# echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> /Users/[your-user-name]/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"
```

It is unclear to me why **Homebrew** would do something like this, whether it is intentional or simply a mistake that hasn't been fixed (at least on my installation). Nonetheless, my shell is now snappier than ever.