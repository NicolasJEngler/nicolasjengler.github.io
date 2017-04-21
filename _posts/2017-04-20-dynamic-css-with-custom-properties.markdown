---
layout: post
title:  "Make Your CSS Dynamic with CSS Custom Properties"
date:   2017-04-20 10:00:00 -0300
categories: 'css'
---

### Originally on [Toptal's Engineering Blog](https://www.toptal.com/front-end/dynamic-css-with-custom-properties)

If you have been writing CSS for a while, you must have at some point in time felt the need for variables. CSS custom properties are somewhat like CSS’s own implementation of variables. However, when used properly, they can be so much more than just variables.

CSS custom properties allow you to:

* Assign arbitrary values to a property with a name of your choice
* Use the var() function to use these values in other properties

Although support for CSS custom properties is a [bit of a rocky path at the moment](http://caniuse.com/#search=custom%20properties), and some browsers support them under flags that need to be activated or set to true beforehand, their support is expected to increase dramatically moving forward, so it’s important to understand how to use and leverage them.

Using CSS custom properties

In this article, you will learn how you can use CSS Custom Properties to make your stylesheets a bit more dynamic, perhaps making that extra Sass/LESS step in your asset pipeline obsolete.

### The Original, and Less Powerful, CSS Variable

Before we begin to discuss CSS custom properties, it should be noted that for a long time now, CSS has had a sort of variable, and that is the `currentColor` keyword. This rarely used but [widely supported](http://caniuse.com/#feat=currentcolor) variable, refers to the current color value of an element. It can be used on any declaration that accepts a `color` value, and it cascades perfectly.

Let’s take a look at an example:

```
.element {
  color: blue;
  border: 2px solid currentColor; /* Sets a solid, 2px wide, blue border to the element */
}
```

In addition to cascading, this can also produce the following:

```
.element span {
  background: currentColor; /* Sets a blue background color for every span child of .element, unless a color property is declared in this same block */
}

.element span.red {
  color: red; /* Sets a red background color for every span child of .element that has the class .red, since currentColor is applied to the background of every span child of .element no matter if they have the .red class or not */
}
```

The main issue with `currentColor`, aside from the fact that it wasn’t in the specification as a variable per se, is that it only accepts the value of the color property, which can make it difficult to work with in some cases.

### Fully-fledged CSS Variables

One of the main advantages of using CSS pre/postprocessors are is that they allow for values to be stored in a keyword and have them scoped to a certain selector if necessary.

After long being requested by developers, a draft for an interpretation of native variables for CSS was written. These are formally referred to as CSS custom properties, but are also sometimes referred to as CSS variables.

The current specification for native CSS custom properties covers all the same behaviors as pre/postprocessor variables. This enables you to store color codes, sizes with all of the known units, or just integers if needed (e.g., when a you need to use the same divisor or multiplier).

The syntax for CSS custom properties is a bit weird compared to other languages, but it makes a whole lot of sense if you compare their syntax with other features in the same CSS ecosystem:

```
:root {
  --color-black: #2e2e2e;
}

.element {
  background: var(--color-black);
}
```

Now, you might be thinking: “What sort of syntax is that!?”

Well, Lea Verou explains the reason for this “dash-dash” syntax with absolute simplicity, as she says in her amazing talk, [CSS Variables: var(–subtitle)](https://www.youtube.com/watch?v=2an6-WVPuJU):

> “They work exactly the same way as any other CSS property […]. So many people ask me why we didn’t use a dollar [sign] or something like that and the reason we didn’t use a dollar [sign] is that we want people to be able to use both SASS, or preprocessor variables and CSS variables. They’re both different things, they accomplish different goals, there are things you can do with CSS variables that you absolutely can not with SASS, and there are things you can do with SASS variables you can not do with CSS variables, so we want people to be able to use both of them in the same stylesheet, so you can imagine the dash-dash syntax as like a prefix property with an empty prefix.”

We can retrieve the value of the custom property using the `var()` function, which we can use everywhere except for selectors, property names, or media query declarations.

It is worth noting that while pre/postprocessor variables are only used at compilation-time, CSS variables can be used and updated dynamically. What does this mean? It means that they are preserved in the actual CSS stylesheet. So the notion that they are variables will remain even after the stylesheets are compiled.

To make it more clear, let me illustrate the situation using some examples. The following block of code is part of a SASS stylesheet:

```
:root {
  $value: 30px;
}

@media screen and (min-width: 768px) {
  $value: 60px;
}

.corners {
  border-radius: $value;
}
```

This snippet of SASS declarations and rules compiles to CSS as follows:

```
.corners {
  border-radius: 30px;
}
```

You can see that both the properties inside `:root` and the media query get lost after compilation, because SASS variables cannot exist inside a CSS file (or, to be more precise, they can be forced to exist in a CSS file, but are ignored since some of their syntax is invalid CSS), so the variable’s value can not be updated afterwards.

Now let’s consider the same case, but applied using only CSS Variables with no CSS pre/postprocessor applied (i.e., without any no transpilation or compilation being performed):

```
:root {
  --value: 30px;
}

@media screen and (min-width: 768px) {
  --value: 60px;
}

.corners {
  border-radius: var(--value);
}
```

Obviously, nothing changes since we haven’t compiled/transpiled anything, and the value of the custom property can be updated dynamically. So, for example, if we change the value of `--value` using something like JavaScript, the value will update in every instance where it is called using the `var()` function.

The capabilities of custom properties make this feature so powerful that you can even do things like autoprefixing.

Lea Verou sets an example using the `clip-path` property. We begin by setting the value of the property we want to prefix to `initial` but use a custom property, and then proceed to set each prefixed property’s value to the custom property value:

```
* {
  --clip-path: initial;
  -webkit-clip-path: var(--clip-path);
  clip-path: var(--clip-path);
}
```

After this, all that’s left is changing the value of the custom property inside a selector:

```
header {
  --clip-path: polygon(0% 0%, 100% 0%, 100% calc(100% - 2.5em), 0% 100%);
}
```

If you’d like to know a bit more about this, check out Lea’s full article on [autoprefixing with CSS variables](http://lea.verou.me/2016/09/autoprefixing-with-css-variables/).

### Bulletproofing CSS Custom Properties

As was mentioned, browser support for CSS Custom Properties is still largely non-standard. So how can this be overcome?

This is where PostCSS, and its plugin, [postcss-css-variables](https://github.com/MadLittleMods/postcss-css-variables), comes into play.

In case you’re wondering what PostCSS is, check my article [PostCSS: SASS’s New Play Date](https://www.toptal.com/front-end/postcss-sass-new-play-date), and come back to this after you’re done. You’ll then have a basic idea of what you can do with this amazing tool and won’t feel disoriented when reading the rest of the article.

With the `postcss-css-variables` plugin, and its `preserve` option set to true, we can keep all the `var()` function declarations in the output and have the computed value as a fallback declaration. It also keeps the computed `--var` declarations. Keep in mind that, using this PostCSS plugin, custom properties can be updated dynamically after the transpilation process, but fallback values will remain the same unless they’re specifically targeted and explicitly changed individually.

If you’re looking for a pre/postprocessor-free way to use CSS variables, you can always check the current support manually with the CSS `@support` rule and apply a proper fallback when support is patchy or non-existent. For example:

```
:root {
  --color-blue: #1e90ff; /* hex value for dodgerblue color */
}

.element {
  background: var(--color-blue);
}

@supports (not(--value: 0)) {
  /* CSS variables not supported */
  .element {
    background: dodgerblue;
  }
}
```

### Changing the Value of a Custom Property Using JavaScript

I’ve been mentioning throughout this whole article that variables can be updated using JavaScript, so let’s get into that.

Say you have a light theme and want to switch it to a dark theme, assuming you have some CSS like the following:

```
:root {
  --text-color: black;
  --background-color: white;
}

body {
  color: var(--text-color);
  background: var(--background-color);
}
```

You can update the `--text-color` and `--background-color` custom properties by doing the following:

```
var bodyStyles = document.body.style;
bodyStyles.setProperty('--text-color', 'white');
bodyStyles.setProperty('--background-color', 'black');
```

### Interesting Use Cases

Over the years of development and discussion regarding the specifications of CSS Custom Properties, some interesting use cases have emerged. Here are a few examples:

**Theming:** Using a set of themes for a site is rather easy when implementing CSS variables. Want a light or dark variation of your current style? Just change the value of some custom properties using JavaScript and you’re done.

**Spacing tune-ups:** Need to fine-tune the spacing of a site, say a gutter between columns? Change the value of a single CSS variable and see this change reflected site-wide.

**Fully dynamic calc() functions:** Now you can have fully dynamic `calc()` functions using custom properties inside these functions, removing the need to make complicated or ephemeral calculations inside JavaScript and then update these values manually on each instance.

### Breathe New Life into Your CSS Files

CSS custom properties are a powerful and innovative way to bring more life to your stylesheets, introducing completely dynamic values for the first time in CSS.

[The spec](https://www.w3.org/TR/2015/CR-css-variables-1-20151203/) is currently under Candidate Recommendation status, which means that standardization is right around the corner, a good reason to dive deep into this feature and get the most out of it.