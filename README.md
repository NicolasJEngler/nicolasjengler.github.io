# nicolasjengler.github.io
Repository dedicated to posts and articles I've written in the past, spread throughout the web, either on sites like CodePen or as an invited author.

## Running locally

The site is built with Jekyll, using the [`github-pages`](https://github.com/github/pages-gem) gem so the local build matches what GitHub Pages deploys.

### Requirements

Ruby 3.1. The `github-pages` gem does not work with the system Ruby 2.6 that ships with macOS.

```sh
brew install ruby@3.1
export PATH="/opt/homebrew/opt/ruby@3.1/bin:$PATH"
```

Add the `export` line to your shell profile (`~/.zshrc`) to make it permanent.

### Install dependencies

```sh
bundle install
```

### Serve

```sh
bundle exec jekyll serve
```

The site is served at http://127.0.0.1:4000/.

### Build only

```sh
bundle exec jekyll build
```

The output goes to `_site/`, which is ignored by git.
