---
layout: post
title:  "CSS transforms and their perspective"
date:   2016-04-08 04:22:00 -0300
categories: 'css'
---

### Originally on [CodePen](http://codepen.io/nicolasjengler/post/css-transforms-and-their-perspective)

So, you've probably seen examples where some DOM elements are rotated on the X/Y/Z axis. This is nothing new as the *transform* property was introduced a relatively long time ago.

CSS *transforms* give us the capability to **rotate**, **scale**, **skew**, and **translate** our elements, The possibilities are endless. But one of the least known properties related to CSS transforms is the *perspective* property.

*perspective* basically turns the "space" in which the object is contained into a 3D plane, or as we already know a three-dimensional Cartesian coordinate system (the same X/Y/Z system we use to **rotate**/**scale**/**skew**/**translate** our elements if we're using all three planes or at least the Z plane). We can see the effects of the *perspective* property in this pen:

<p data-height="400" data-theme-id="0" data-slug-hash="bpaadm" data-default-tab="css,result" data-user="nicolasjengler" data-embed-version="2" data-pen-title="CSS perspective demo" class="codepen">See the Pen <a href="http://codepen.io/nicolasjengler/pen/bpaadm/">CSS perspective demo</a> by Nicols J Engler (<a href="http://codepen.io/nicolasjengler">@nicolasjengler</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

Notice a few things:

1. The *perspective* property is applied to the parent of the children that we want to be three-dimensional. Meaning that its main difference with *transform*'s own *perspective()* function (yes, the *transform* property also has possible value which is said function) is that it applies the 3D Cartesian coordinate system to **ALL** of it descendants, while the *perspective()* function only applies the previously named coordinate system to the element.
2. The higher the value of our *perspective* property the closer we get to the element, meaning that whatever transformation we apply to it may be more noticeable if we set a small value. This is better seen when applying rotational transforms to our elements.
3. We specified a value for our element's *transform-origin* property. Isn't this a bit confusing keeping in mind that now we have our parent's *perspective* property tampering our **transformed elements**? If you were asking yourself that, then you're correct, and we'll talk a bit more about it below.

As you may have probably guessed, the *perspective* property has a companion which is the, drum roll please, *perspective-origin* property. The *perspective-origin* property is the one in charge to set where our _vanishing point_ is, or to put it simply it determines from what angle we're looking at our elements. In the pen below, we can see our previous example usage of the *perspective* property in combination with the *perspective-origin* property, on the left the value is set to *center left* and on the right the value is set to *center right*.

<p data-height="400" data-theme-id="0" data-slug-hash="aNEEpB" data-default-tab="css,result" data-user="nicolasjengler" data-embed-version="2" data-pen-title="CSS perspective & perspective-origin demo" class="codepen">See the Pen <a href="http://codepen.io/nicolasjengler/pen/aNEEpB/">CSS perspective & perspective-origin demo</a> by Nicols J Engler (<a href="http://codepen.io/nicolasjengler">@nicolasjengler</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

If we wanted to, we could get rid of both element's own *transform-origin* value (set it back to *50% 50%*) and work with the parent's *perspective-origin* to achieve the same results as in the first pen, but for demonstrational reasons we're using both *transform-origin* and *perspective-origin* properties in this last pen.

**Note:** the *perspective* property isn't a shorthand that includes our previously mentioned *perspective-origin* property, they both have to be declared separately when writing our CSS.

Finally, let's add another layer of complexity to the beloved CSS 3D space we're currently working on. If we're working with transformations on the Z plane (most likely stacking content on different levels) and you rotate the element on it's Y axis, you may notice that all of sudden you have no layers and everything seems... **flat**. That is because our CSS *transform-style* property is set to a *flat* value by default. This property basically defines if children elements are in a 3D space or a 2D space, and it has 2 possible values *flat* and *preserve-3d*, both are pretty self-explanatory. On the pen below, created by [Louis Lazaris](http://codepen.io/impressivewebs), we can check the *transform-style* property in action.

<p data-height="400" data-theme-id="0" data-slug-hash="pIgBf" data-default-tab="css,result" data-user="impressivewebs" data-embed-version="2" data-pen-title="pIgBf" class="codepen">See the Pen <a href="http://codepen.io/impressivewebs/pen/pIgBf/">pIgBf</a> by Louis Lazaris (<a href="http://codepen.io/impressivewebs">@impressivewebs</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>


* [perspective](https://css-tricks.com/almanac/properties/p/perspective/) by *CSS-Tricks*
* [perspective-origin](https://developer.mozilla.org/en-US/docs/Web/CSS/perspective-origin) by *Mozilla Developer Network*
* [transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform) by *Mozilla Developer Network*
* [transform-style](https://css-tricks.com/almanac/properties/t/transform-style/) by *CSS-Tricks*
