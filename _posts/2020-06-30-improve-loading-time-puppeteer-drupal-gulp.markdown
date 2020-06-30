---
layout: post
title:  "How to improve loading time performance with Gulp and Puppeteer on a Drupal site"
date:   2020-06-30 01:00:00 -0300
categories: 'development'
---

### Originally on [Dev.to](https://dev.to/nicolasjengler/how-to-improve-loading-time-performance-with-gulp-and-puppeteer-on-a-drupal-site-653)

By now **Drupal** is a fairly known content management framework, which is why some companies choose to build their site using it given its history and how robust it is. 

I've been working for a client for about 2 years now, and they have a rather large Drupal site with multiple dependencies. For this client in particular I'm in charge of front-end development and some back-end integrations, and currently we're using a **Gulp**-based workflow to manage static assets. This process involves **Sass** compilation, image compression, and JS minification/concatenation, among other stuff.

In a routine check, a member of the client's team decided to run the site through Google's **PageSpeed Insights** and to my dismay our initial score was pretty low, ranging between 20 and 30. After this report I decided to dig in a bit deeper and see how we could improve our PSI score which led to some interesting takeaways. Not only we were using a bunch of third party scripts for different tasks (some scripts weren't even necessary anymore) but we also realised that Drupal tends to position render-blocking content at the top of the page, inside the `head` tag, that could be either deferred, preloaded or moved to the bottom of the DOM right before the closing `body` tag.

![Low PageSpeed Insights score](https://dev-to-uploads.s3.amazonaws.com/i/5cgbdewtxjecs7jqll93.png)

But moving our render-blocking content to the bottom of the document wasn't enough, since we were now getting pretty awful performance on metrics like our **First Meaningful Paint**. Based on this we decided to see if there was a proper way to create critical CSS and include said declarations inline in the head of the DOM, this would help us improve our FMP and **perceived loading times** keeping the benefits of moving the rest of our render blocking resources to the end.

## Approach No. 1: Hand-picked critical CSS

Our first thought when moving forward to create critical CSS rules to include on the site was to generate a hand-crafted separate file. This process was running smoothly until we tried to import some partial Sass that depended on **Bootstrap** mixins and variables, which eventually led to dependency hell defeating the purpose of critical CSS. We were unable to create a critical CSS file since we were including a bunch of unneeded declarations because of dependencies.

![Dependency hell using custom Sass file](https://dev-to-uploads.s3.amazonaws.com/i/be6fi3sygxhu4gvepr0r.png)

## Approach No. 2: Fetch the homepage critical CSS using a tool like Chrome/Chromium DevTools' Code Coverage

After learning about Chrome/Chromium DevTools' Code Coverage we thought *"What if we could run a headless browser when the build process runs and use the DevTools to fetch our homepage's actually used CSS which also includes stuff like navbar, menu, text sizing and color, etcetera?"*

Enter **Puppeteer**: Puppeteer is a Node library which provides a high-level API to control Chrome or Chromium over the DevTools Protocol. Puppeteer runs headless by default, but can be configured to run full (non-headless) Chrome or Chromium.

The first step to include Puppeteer in our workflow was to add it as a dependency:

```bash
npm install --save-dev puppeteer
```

And then we include the dependency in our `gulpfile.js`

```js
const puppeteer = require('puppeteer');
```

After Puppeteer is available to work within our Gulpfile we proceed to create a new task (named `css-critical`) in charge of generating the critical CSS file and declare a variable to hold the URL from which Puppeteer will fetch our critical CSS:

```js
gulp.task('css-critical', async function() {
    const URL = 'https://exampleurl.com';
});
```

With that in place, we now need to declare a new empty string variable to hold whatever we gather as critical CSS, and launch a headless browser with a viewport of 1440x900 pixels:

```js
gulp.task('css-critical', async function() {
    const URL = 'https://exampleurl.com';
    let criticalCSS = '';

    const browser = await puppeteer.launch({
        headless: true,
        args: [`--window-size=1440,900`],
        defaultViewport: null
    });
});
```

Our next step is to open a new page, start the CSS Coverage tool, load our site, store the results in a variable called `cssCoverage` and finally stop the CSS coverage tool.

```js
gulp.task('css-critical', async function() {
    const URL = 'https://exampleurl.com';
    let criticalCSS = '';

    const browser = await puppeteer.launch({
        headless: true,
        args: [`--window-size=1440,900`],
        defaultViewport: null
    });
    const page = await browser.newPage();

    await page.coverage.startCSSCoverage();
    await page.goto(URL, {waitUntil: 'load'})

    const cssCoverage = await page.coverage.stopCSSCoverage();
});
```

Next we'll need to select the used data ranges returned by the Coverage tool in order to compose our final CSS file.

```js
gulp.task('css-critical', async function() {
    const URL = 'https://exampleurl.com';
    let criticalCSS = '';

    const browser = await puppeteer.launch({
        headless: true,
        args: [`--window-size=1440,900`],
        defaultViewport: null
    });
    const page = await browser.newPage();

    await page.coverage.startCSSCoverage();
    await page.goto(URL, {waitUntil: 'load'})

    const cssCoverage = await page.coverage.stopCSSCoverage();

    for (const entry of cssCoverage) {
        for (const range of entry.ranges) {
        criticalCSS += entry.text.slice(range.start, range.end) + "\n"
        }
    }
});
```

![Example of Chrome's DevTools CSS Coverage](https://dev-to-uploads.s3.amazonaws.com/i/6lwtt6gt15qhvqh1prpg.png)

After this is done and ready, we'll proceed to close the page, close the browser and dump the content of our `criticalCSS` into an actual file, that will later be inlined into our Drupal `html.html.twig` template.

```js
gulp.task('css-critical', async function() {
    const URL = 'https://exampleurl.com';
    let criticalCSS = '';

    const browser = await puppeteer.launch({
        headless: true,
        args: [`--window-size=1440,900`],
        defaultViewport: null
    });
    const page = await browser.newPage();

    await page.coverage.startCSSCoverage();
    await page.goto(URL, {waitUntil: 'load'})

    const cssCoverage = await page.coverage.stopCSSCoverage();

    for (const entry of cssCoverage) {
        for (const range of entry.ranges) {
        criticalCSS += entry.text.slice(range.start, range.end) + "\n"
        }
    }

    await page.close();
    await browser.close();

    require('fs').writeFileSync('css/critical.css', criticalCSS);
});
```

With everything in place, all that's left to do is to inject our critical CSS file into our template and move all render-blocking CSS and JS to the bottom of our DOM. `html.html.twig` should end up looking something like this:

```php
<!DOCTYPE html>
<html{{ html_attributes }}>
  <head>
    ...
    <style media="screen">
        {% include directory ~ '/css/critical.css' ignore missing %}
    </style>
    <js-placeholder token="{{ placeholder_token|raw }}">
  </head>
  <body>
    ...
    <css-placeholder token="{{ placeholder_token|raw }}">
    <js-bottom-placeholder token="{{ placeholder_token|raw }}">
  </body>
</html>
```

And that's it! This approach helped us move our PageSpeed Insights score between 50 and 60 points up from the initial 20-30 we were getting.

![Higher PageSpeed Insights score after applying changes](https://dev-to-uploads.s3.amazonaws.com/i/07nn01iqj3hpyfxdygim.png)

### Some improvements that can potentially be done:

1. Remove duplicate declarations by comparing critical CSS generated and regular CSS
2. Remove unwanted elements that might not be considered critical for the site, i.e.: a slider, video decoration, animations
3. Create a page-by-page approach to serve critical CSS that's adjusted for each page rather than just one page used in general