---
layout: post
title:  "Little did they know they had pretty similar jobs..."
date:   2019-04-21 21:00:00 -0300
categories: 'development'
---

### Originally on Indicius' Medium publication

### Or how front-end developers and designers understood which parts of collaboration are key to excel at their work.

This is the story of position meets position, more accurately front-end devs and UI/UX designers meeting each other's jobs implications to further understand and nurture healthy collaboration. For "healthy collaboration" I mean avoiding endless feedback rounds, teeth grinding, and "that's misaligned 10px to the left" or "works on my machine".

Last month I had the chance to travel to Buenos Aires and lead a workshop organized in collaboration with [Indicius](https://medium.com/@Indicius). I've been meaning to do a presentation or organizing a workshop for a while now, and Hernán (Indicius' founder and CEO) knew about this, which is why he reached out asking if I was interested in taking part of an initiative that was looking to improve the exchange that is born when a design deliverable is handed to developers. Given that my years at Indicius were some of my most interesting ones as a front-end developer, I knew that this was the collaboration I had been waiting for.

After coordinating dates, topics, surveying Indicius' amazing team (and other third parties), and getting a presentation ready, we ended up with a workshop of two days: a day to do some theoretic introduction, and a day of playground exploration.

#### Part 1: Designers and front-end devs: siblings separated at birth

The first part basically dived into how designers and front-end developers can understand each other's work, comprehend what's similar and what's totally different, and later use that knowledge to get collaboration working like a well-oiled machine rather than pulling each other's hair off

#### Part 2: HTML - HyperText Markup Language or How To Make Little websites

The second part was mostly an exploratory section of how HTML works, basic syntax, what it was created for, where to get reliable documentation, how can designers think of HTML elements and groups of HTML elements as symbols or components of their UI/UX design tool of choice. This part included a short amount of time dedicated to writing basic HTML elements and playing with them to see how syntax changes tiny bits of the output in the browser.

On a different tone, part 2 also included some discussion of the topic that weighs on GUI-based tools to create websites versus coding. Why coding when we already have apps and tools with graphical user interfaces to create websites? Short answer: it depends on what you're looking for. GUI-based tools are great for speedy projects with low-level customization, while coding allows for granular control of assets, styles and interactions.

Part 2 also touched a common exchange with prospect clients: dynamic vs static website. What's the added workload to create dynamic websites rather than just editing an HTML document? Static websites are pretty straight forward, write some code, check it out in a browser and watch the result, the caveat is that in order to update its content you'll have to do it manually by editing the HTML document. On the other side of the spectrum, dynamic websites add a layer of complexity that requires several extra steps with the advantage of being able to edit the site through some kind of interface.

#### Part 3: CSS - Cascading Style Sheets or Code Super Styles

Part 3 was the most interesting in my humble opinion. What do developers do with CSS and what are its limitations? A short intro to CSS' own box model and what I like to call "n-level limitations". Similarities and differences between UI/UX design tools and the specifications of CSS. CSS has a box model in order to organize different properties when styling an element, i.e.: margin, border and padding. This help us understand, for example, where background will be clipped, or where there'll be spacing in between elements when using margins. 

CSS basically allows us to style whatever we want in a page, give it a determined size, background color, drop shadow, border, rounded corners, flow, etcetera. HTML alone looks pretty straight forward: a basic document with little to no style that flows from top to bottom, while HTML + CSS allow us to turn that into something more unique.

With similarities, discrepancies and n-level limitations for CSS, we need to understand why it is good to prioritize pragmatic implementation over pixel perfection:

* Graceful degradation avoids polyfill libraries which in turn leaves you with a lighter codebase
* Native solutions mean less buggy content and completely accesible documentation
* Implementation time is way smaller when there's no need for third-party library implementation
* Reasonably expectations on delivery

This part included a short amount of time dedicated to writing basic CSS in combination to the previously written HTML elements and playing with CSS properties to see how this styles tiny bits of HTML elements in the browser.

#### Part 4: JS - JavaScript or Just Shake things up

This was just an overview section given that the current JavaScript ecosystem is incredibly complex on its own. Nonetheless I wanted to go through some of the basics of JavaScript implementations and common usage on a day to day basis. 

What does JavaScript do? Whenever used for front-end development, JavaScript allows developers to extend the native functionality that HTML and CSS provide. Interactivity is the main use for JS in front-end development but its usage in the development world varies from webservers to package management or task running.

**Common usage for the web and assets**

* Creating and triggering modals and popups
* Triggering CSS animations, or animating complex scenes
* State-changes in UIs: changing the data, and the UI, seen on screen while keeping the data structure * * files and databases up-to-date.
* Minification of files to carry a lighter load
* Concatenation of files to make fewer requests

**Common tools**

- **Gulp:** toolkit for automating time-consuming tasks in a development workflow
- **Grunt:** a task runner used to automatically perform frequent tasks such as minification, compilation, unit testing, and linting (deprecated)
- **Webpack:** an open-source module bundler. Its main purpose is to transform, bundle, or package files for usage in a browser
- **Bower:** a package manager for third party libraries. Good examples are CSS frameworks like Bootstrap, or libraries like jQuery (deprecated)
- **Yarn:** a new package manager that replaces the existing workflow for the npm client or other package managers while remaining compatible with the npm registry
- **npm:** a package manager for Node.js. It consists of a command line client, and an online database of public and paid private packages

Each library has its own purpose and thus its own level of complexity. The libraries that usually interest the most to designers are the ones that give the capability to create visually appealing experiences. A library that allows shape morphing or meshing might be a bit more complex than a library used to add tabulation capabilities like filtering and/or column toggling. And with this in mind we closed the last part of the workshop with an open Q&A section!

I have to say, I had a blast while fulfilling this experience. Meeting with former colleagues and having the chance to share some of the tricks and knowledge I've been collecting in the past 9 years or so was magic ✨

Huge shout out to [Indicius](https://medium.com/@Indicius) for giving me the chance to collaborate and get this done. I will be forever thankful!