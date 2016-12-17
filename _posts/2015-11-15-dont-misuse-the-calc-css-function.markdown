---
layout: post
title:  "Don't misuse the calc() CSS function!"
date:   2015-11-17 21:26:00 -0300
categories: css
---

### Originally on [CodePen](http://codepen.io/nicolasjengler/post/don-t-misuse-the-calc-css-function)

Lately I've been using **calc()** more and more, but here's the thing: using calc() excessively is wrong, and using calc() for things that it is not supposed to is also wrong. Wrong as in using !important in your CSS, because it usually means that there's something out of place, something you're missing.

I've compiled some situations where calc is used, and then **I labeled them according to what I believe** are proper and inconvenient use cases. Again, this categorization was done by myself, so I may not be right on every case.
If you think that there's something in this post that can be corrected, please leave a comment and I'll make sure to check it out 😁

## Incorrect use of calc()

### Using *calc() instead of the box-sizing property*
It's now sort of common to see people combining calc() with block level elements that have a border. For example, assuming we have a **parent** that has a **fixed width**, and we have **two children** that are **50% of its width** and have a border with a **thickness of 15px**, what some people would do is set its **width** value to something like **calc(50% - 30px)** (30px being the sum of its left-border and its right-border).
This is a no-no. There's a property called **box-sizing** (check its documentation [right here](https://developer.mozilla.org/en/docs/Web/CSS/box-sizing)) and it was specifically designed to take *paddings and borders* into account when calculating an element's rendered width or height.

### As a *"replacement" for flexbox*
Two days ago I read someone ranting on Twitter that calc() is basically a *reduced flexbox layout mode*. **Nope**, nope, and nope. If you think that calc() is powerful enough on its own to compare it to the whole flexbox system, I advise you to go and check [Wes Bos](http://wesbos.com/)' ["What the flexbox!?"](http://flexbox.io/). "What the flexbox!?" is an amazing set of videos to help you grasp a better understanding of how **powerful** the flexbox layout system is.

## Correct use of calc()

### Mixing *fixed-width elements with relative-width* ones
Let's take as an example a website that has a *fixed-width* sidebar, let's say something like 300px. If we want to make this website responsive, we could set the main content's **width** to something like **calc(100% - 300px)**. Now assuming that its parent container is the **body** element, our main content would adapt to 100% of its parent while the sidebar would stick to its 300px width.

### Positioning *background images from the bottom-right corner*
Setting a background image position from the top-left corner is a piece of cake. The **background-position** property is already set to function that way. Setting this property to a value like **20% 30%** positions the element 20% from the left side and 30% from the top, **but** using something like **calc(100% - 20%) calc(100% - 30%)** would position the image 20% from the right side and 30% from the bottom.

_**EDIT**_: as pointed out by [@dylanjameswagner](http://codepen.io/dylanjameswagner/), this use case isn't correct either. The **background-image** property also accepts a **3 to 4 value** syntax that allows background images to be positioned from the right side and the bottom in a native way. More info about it [right here](http://caniuse.com/#feat=css-background-offsets). Thanks for the heads up, Dylan! 😉

### Setting a *block level element to be as tall as its block level parent while having another element in the way*
Let's imagine we have a *parent element* that is 500px tall, and inside it we have *two more elements* one that is 50px tall and the other we want it have a **height** value of **100%**. The issue here is that the second child would be 500px tall (because it would be as tall as its parent element) and the addition of those 500px plus the 50px of the first child gives us a total height of *550px*, resulting in a content overflow (again, this happens because our parent element is only *500px* tall). We could solve this issue by making the second child to have a **height** value of **calc(100% - 50px)**, and *voilà*, problem solved.

------

 - [calc()](https://developer.mozilla.org/en-US/docs/Web/CSS/calc) by _Mozilla Developer Network_
 - [box-sizing](https://developer.mozilla.org/en/docs/Web/CSS/box-sizing) by _Mozilla Developer Network_
 - [flexbox](https://developer.mozilla.org/en/docs/Web/CSS/flex) by _Mozilla Developer Network_
 - [What the flexbox!?](http://flexbox.io/) by _Wes Bos_
 - [background-position edge offsets](http://caniuse.com/#feat=css-background-offsets) by _Can I Use?_

*__Note__: element flotation and similar stuff is avoided (unless absolutely necessary) in order to make the examples way simpler*
