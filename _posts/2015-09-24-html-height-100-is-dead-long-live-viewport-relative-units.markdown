---
layout: post
title:  "html { height: 100% } is dead, long live viewport-relative units!"
date:   2015-09-24 23:14:00 -0300
categories: 'css'
---

### Originally on [CodePen](http://codepen.io/nicolasjengler/post/html-height-100-is-dead-long-live-viewport-relative-units)

In this day and age we're living it is very common to make the information that's above the fold very important and impressive. I mean, it makes a lot of sense since we need to get the data that's critical for the user to be the first thing that shows up in our site.

In order to create a fullscreen block I used to set the __*html*__ and __*body*__ tags' __*height*__ to a value of at least __*100%*__, afterwards I would create a _div_ wrapping all of the content and also set its height to _100%_. This is required because without the _root element_ (a.k.a. _html_ tag) __AND__ the _body_ tag's height set to _100%_ (or a fixed value in other cases, I'm using _100%_ because of the fullscreen block thing), setting a children's height to a percentage-based value won't work. For some reason a children element can't have a percentage-based height if the parent element's height is set to _auto_)

There's also _something important to note_ here. Using a property like min-height will give the same results as using the auto value in the height property. Check out this demo I've created in order to show this:

<p data-height="400" data-theme-id="0" data-slug-hash="vNXvxQ" data-default-tab="css,result" data-user="nicolasjengler" data-embed-version="2" data-pen-title="Percentage-based heights" class="codepen">See the Pen <a href="http://codepen.io/nicolasjengler/pen/vNXvxQ/">Percentage-based heights</a> by Nicols J Engler (<a href="http://codepen.io/nicolasjengler">@nicolasjengler</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

### Enter viewport-relative units

Viewport-relative units have been around for a while, but now's the time that they're getting the biggest support yet. If you check [this table](http://caniuse.com/#feat=viewport-units) (courtesy of our friends at [__*caniuse.com*__](http://caniuse.com)), you'll see that these units are well supported on all the latest versions of major browser, having only partial support on _IE/Edge_, and no support at all only in _Opera Mini_. This is a great way towards full implementation.

_What do viewport-relative units do?_ That's a great question, I'm glad you asked. Viewport-relative units are units that represent 1% of the viewports width or height, _vh_ standing for __viewport's height__, and _vw_ standing for __viewport's width__. There's also _vmin_ that uses the viewports smallest value (regardless it's width _OR_ height), same for _vmax_ except this one uses the biggest value.

So if I set an element's height to __*50vh*__, it's height will be 50%, relative to the viewport's height. Same goes for width values. See these units in action in this next pen:

<p data-height="400" data-theme-id="0" data-slug-hash="QjKzJG" data-default-tab="css,result" data-user="nicolasjengler" data-embed-version="2" data-pen-title="Viewport relative units" class="codepen">See the Pen <a href="http://codepen.io/nicolasjengler/pen/QjKzJG/">Viewport relative units</a> by Nicols J Engler (<a href="http://codepen.io/nicolasjengler">@nicolasjengler</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

__My point being__... viewport-relative units provide a sleek, painless, way to create fullpage above the fold elements, and they're a pretty interesting thing to experiment with (check [this thing](https://css-tricks.com/viewport-sized-typography/) out, for example). To learn more about them, check these sources out:

* [Viewport units: vw, vh, vmin, vmax](https://web-design-weekly.com/2014/11/18/viewport-units-vw-vh-vmin-vmax/) by _Web Design Weekly_
* [New viewport-relative units](http://blog.teamtreehouse.com/new-viewport-relative-units) by _TreeHouse_
