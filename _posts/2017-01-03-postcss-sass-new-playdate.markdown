---
layout: post
title:  "PostCSS: Sass’s New Play Date"
date:   2017-01-03 10:00:00 -0300
categories: 'workflow'
---

### Originally on [Toptal's Engineering Blog](https://www.toptal.com/front-end/postcss-sass-new-play-date)

Lately, [PostCSS](http://postcss.org/) is the tool making the rounds on the [front-end side of web development](https://www.toptal.com/front-end).

PostCSS was developed by Andrey Sitnik, the creator of [Autoprefixer](https://github.com/postcss/autoprefixer). It is a Node.js package developed as a tool to transform all of your CSS using JavaScript, thereby achieving much faster build times than other processors.

Despite what its name seems to imply, it is not a post-processor (nor is it a pre-processor), but rather it is a transpiler to turn PostCSS-specific (or PostCSS plugin-specific, to be more precise) syntax into vanilla CSS.

With that being said, this does not mean that PostCSS and other CSS processors can’t work together. As a matter of fact, if you’re new to the whole world of CSS pre/post-processing, using PostCSS along with Sass can save you many headaches, which we’ll get into shortly.

PostCSS is not a replacement for other CSS processors; rather, look at it as another tool that may come in handy when needed, another addition to your toolset.

Use of PostCSS has recently begun to increase exponentially, with it being used today by some of the biggest tech industry businesses, like Twitter, JetBrains, and Wikipedia. Its widespread adoption and success is largely due to its modularity.

### Plugins, Plugins, Plugins

PostCSS is all about plugins.

It allows you to choose the plugins you will use, ditching unneeded dependencies, and giving you both a quick and lightweight setup to work with, with the basic characteristics of other preprocessors. Also, it allows you to create a more heavily customized structure for your workflow while keeping it efficient.

As of the date of writing of this post, PostCSS has a repository of more than 200 plugins, each of them in charge of different tasks. On the [PostCSS’ GitHub repository](https://github.com/postcss/postcss), plugins are categorized by “Solve global CSS problems,” “Use future CSS, today,” “Better CSS readability,” “Images and fonts,” “Linters,” and “Others.”

However, in the [plugins directory](https://github.com/postcss/postcss/blob/master/docs/plugins.md) you will find a more accurate categorization. I advise you take a look there yourself to get a better idea of the capabilities of a few of them; they are quite broad and rather impressive.

You’ve probably heard of the most popular PostCSS plugin, [Autoprefixer](https://github.com/postcss/autoprefixer), which is a popular standalone library. The second most popular plugin is [CSSNext](https://github.com/MoOx/postcss-cssnext), a plugin that allows you to use the latest CSS syntax today, such as the CSS’ new custom properties, for example, without worrying about the browser support.

Not all PostCSS plugins are so groundbreaking though. Some simply give you capabilities that probably come out of the box with other processors. Take the `parent selector` for example. With Sass, you can start using it immediately as you install Sass. With PostCSS, you need to use the `postcss-nested-ancestors` plugin. The same could apply to `extends` or `mixins`.

So, what’s the advantage of using PostCSS and its plugins? The answer is simple - you can pick your own battles. If you feel like the only part of Sass you’re ever going to use is the parent selector, you can save yourself the stress of implementing something like a Sass library installation in your environment to compile your CSS, and speed up the process by using only PostCSS and the postcss-nested-ancestors plugin.

That is just one example use case for PostCSS, but once you start checking it out yourself, you’ll undoubtedly realize many other use cases for it.

### Basic PostCSS Usage

First, let’s cover some PostCSS basics and take a look how it is typically used. While PostCSS is extremely powerful when used with a task runner, like Gulp or Grunt, it can also be used directly from the command line by using the [postcss-cli](https://github.com/postcss/postcss-cli).

Let’s consider a simple example use case. Assume we’d like to use the [postcss-color-rgba-fallback](https://github.com/postcss/postcss-color-rgba-fallback) plugin in order to add a fallback HEX value to all of our RGBA formatted colors.

Once we’ve NPM installed `postcss`, `postcss-cli` and `postcss-color-rgba-fallback`, we need to run the following command:

```
postcss --use postcss-color-rgba-fallback -o src/css/all.css dist/css/all.css
```

With this instruction, we’re telling PostCSS to use the `postcss-color-rgba-fallback` plugin, process whatever CSS is inside `src/css/all.css`, and output it to `dist/css/all.css`.

OK, that was easy. Now, let’s look at a more complex example.

### Using PostCSS Along with Task-runners and Sass

PostCSS can be incorporated into your workflow rather easily. As mentioned already, it integrates perfectly well with task runners like Grunt, Gulp, or Webpack, and it can even be used with NPM scripts. An example of using PostCSS along with Sass and Gulp is as simple as the following code snippet:

```
var gulp = require('gulp'),
    concatcss = require('gulp-concat-css'),
    sass = require('gulp-sass'),
    postcss = require('gulp-postcss'),
    cssnext = require('postcss-cssnext');

gulp.task('stylesheets', function () {
  return (
    gulp.src('./src/css/**/*.scss')
    .pipe(sass.sync().on('error', sass.logError))
    .pipe(concatcss('all.css'))
    .pipe(postcss([
      cssNext()
    ]))
    .pipe(gulp.dest('./dist/css'))
  )
});
```

Let’s deconstruct the above code example.

It stores references to all of the needed modules (Gulp, Contact CSS, Sass, PostCSS, and CSSNext) in a series of variables.

Then, it registers a new Gulp task called `stylesheets`. This task watches for files that are in `./src/css/` with the extension `.scss` (regardless of how deep in the subdirectory structure they are), Sass compiles them, and concatenates all of them to a single all.css file.

Once the `all.css` file is generated, it is passed to PostCSS to transpile all of the PostCSS (and plugin) related code to the actual CSS, and then the resulting file is placed in `./dist/css`.

OK, so setting up PostCSS with a task runner and a preprocessor is great, but is that enough to justify working with PostCSS in the first place?

Let’s put it like this: While Sass is stable, mature, and has a huge community behind it, we might want to use PostCSS for plugins like Autoprefixer, for example. Yes, we could use the standalone Autoprefixer library, but the advantages of using Autoprefixer as a PostCSS plugin is the possibility to add more plugins to the workflow later on and avoid extraneous dependencies on a boatload of JavaScript libraries.

This approach also allows us to use unprefixed properties and have them prefixed based on the values fetched from APIs, like the one from [Can I Use](http://caniuse.com/), something that is hardly achievable using Sass alone. This is pretty useful if we’re trying to avoid complex mixins that [might not be the best way to prefix code](https://twitter.com/autoprefixer/status/723162483309998080).

The most common way to integrate PostCSS into your current workflow, if you’re already using Sass, is to pass the compiled output of your `.sass` or `.scss` file through PostCSS and its plugins. This will generate another CSS file that has the output of both Sass and PostCSS.

If you’re using a task runner, using PostCSS is as easy as adding it to the pipeline of tasks you currently have, right after compiling your `.sass` or `.scss` file (or the files of your preprocessor of choice).

PostCSS plays well with others, and can be a relief for some major pain points we as developers experience every day.

Let’s take a look at another example of PostCSS (and a couple of plugins likes CSSNext and Autoprefixer) and Sass working together. You could have the following code:

```
:root {
    $sass-variable: #000;
    --custom-property: #fff;
}

body {
    background: $sass-variable;
    color: var(--custom-property);

    &:hover {
        transform: scale(.75);
    }
}
```

This snippet of the code has vanilla CSS and Sass syntax. [Custom properties](https://drafts.csswg.org/css-variables/), as of the day of the writing of this article, are still in Candidate Recommendation (CR) status, and here’s where the CSSNext plugin for PostCSS comes into action.

This plugin will be in charge of turning stuff like custom properties into today’s CSS. Something similar will happen to the `transform` property, which will be auto-prefixed by the Autoprefixer plugin. The code written earlier will then result in something like:

```
body {
    background: #000;
    color: #fff;
}

body:hover {
    -webkit-transform: scale(.75);
            transform: scale(.75);
}
```

### Authoring Plugins for PostCSS

As mentioned earlier, an attractive feature of PostCSS is the level of customization it allows. Thanks to its openness, authoring a custom plugin of your own for PostCSS to cover your particular needs is a rather simple task if you’re comfortable writing JavaScript.

The folks at PostCSS have a pretty solid list to start, and if you’re interested in developing a plugin check their [recommended articles and guides](https://github.com/postcss/postcss/blob/master/docs/writing-a-plugin.md). If you feel like you need to ask something, or discuss anything, [Gitter](https://gitter.im/postcss/postcss) is the best place to start.

PostCSS has [its API](http://api.postcss.org/) with a pretty active base of followers on [Twitter](https://twitter.com/postcss). Along with other community perks mentioned earlier in this post, this is what make the plugin creation process so much fun and such a collaborative activity.

So, to create a PostCSS plugin, we need to create a Node.js module. (Usually, PostCSS plugin folders in the `node_modules/` directory are preceded by a prefix like “postcss-”, which is to make it explicit that they are modules that depend on PostCSS.)

For starters, in the `index.js` file of the new plugin module, we need to include the the following code, which will be the wrapper of the plugin’s processing code:

```
var postcss = require('postcss');

module.exports = postcss.plugin('replacecolors', function replacecolors() {
  return function(css) {
    // Rest of code
  }
});
```

We named the plugin *replacecolors*. The plugin will look for a keyword `deepBlackText` and replace it with the `#2e2e2e` HEX color value:

```
var postcss = require('postcss');

module.exports = postcss.plugin('replacecolors', function replacecolors() {
  return function(css) {
    css.walkRules(function(rule) {
      rule.walkDecls(function(decl, i) {
		var declaration = decl.value;
        if (declaration.indexOf('deepBlackText') !== -1) {
          declaration = ‘color: #2e2e2e;’;
        }
      });
    });
  }
});
```

The previous snippet just did the following:

1. Using `walkRules()` it iterated through all of the CSS rules that are in the current .css file we’re going through.

2. Using `walkDecls()` it iterated through all of the CSS declarations that are inside the current .css file.

3. Then it stored the declaration inside the declaration variable and checked if the string `deepBlackText` was in it. If it was, it replaced the whole declaration for the following CSS declaration: `color: #2e2e2e;`.

Once the plugin is ready, we can used it like this directly from the command line:

```
postcss --use postcss-replacecolors -o src/css/all.css dist/css/all.css
```

Or, for example, loaded in a Guplfile like this:

```
var replacecolors = require('postcss-replacecolors');
```

### Should I Ditch My Current CSS Processor in Order to Use PostCSS?

Well, that depends on what you’re looking for.

It is common to see both Sass and PostCSS used at the same time, since it is easier for newcomers to work with some of the tools that pre/post-processors offer out of the box, along with PostCSS plugins’ features. Using them side-by-side can also avoid rebuilding a predefined workflow with relatively new, and most likely unknown, tools, while providing a way to maintain current processor-dependant implementations (like Sass mixins, extends, the parent selector, placeholder selectors, and so on).

### Give PostCSS a Chance

PostCSS is the hot (well, sort of) new thing in the front-end development world. It has been widely adopted because it is not a pre/post-processor per se, and it is flexible enough to adapt to the environment it is being inserted into.

Much of the power of PostCSS resides in its plugins. If what you’re looking for is modularity, flexibility, and diversity, then this is the right tool for the job.

If you’re using task runners or bundlers, then adding PostCSS to your current flow will most likely be a piece of cake. Check the installation and usage guide, and you will probably find an easy way to integrate it with the tools you’re already using.

Many developers say it is here to stay, at least for the foreseeable future. PostCSS can have a great impact on how we structure our present-day CSS, and that could potentially lead to a much greater adoption of standards across the front-end web development community.
