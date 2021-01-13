---
layout: post
title:  "I can still see it: a case for graceful degradation"
date:   2021-01-13 20:00:00 -0300
categories: 'development'
---

### Originally on [Dev.to](https://dev.to/nicolasjengler/i-can-still-see-it-a-case-for-graceful-degradation-5ef1)

There are plenty of pieces written about how we should **strive for progressive enhancement and leave graceful degradation behind**, and I understand the case for that given how the current front-end development ecosystem works, and the status of the tools we're currently given to work on a day-to-day basis. 

One of the main reasons I'm so uncomfortable working with junior developers is that they are at a loss when you start explaining to them how package managers work, since **we're no longer toying with just HTML, CSS and JavaScript files**. Third-party dependencies are often needed for progressive enhancement to be achieved. Juniors are unaware of what Node.js and npm are, how to get them (don't even get me started on the differences between OSes), how to use them on their project. **The learning curve is steep and progressive enhancement requires fully-fledged scripts that understand the bells and whistles of a product**, or an entire dependency that takes care of that. 

It's understandable for larger companies or larger teams to have the time to train their juniors to apply this approach, or have a dedicated developer working on it, but for smaller team it is additional pressure on deadlines and there's an awful lot of guilt-tripping from peers when you decide to go for graceful degradation rather than take the time to progressively enhance a feature or product.

If we head over to the [W3 Wiki](https://www.w3.org/wiki/Graceful_degradation_versus_progressive_enhancement) on **Graceful Degradation vs Progressive Enhancement**: 

> Your product by definition is so dependent on scripting that it makes more sense to maintain a “basic” version rather than enhancing one (Maps, email clients, feed readers)

That's how it works for smaller teams. I realized this while talking to a client that needed to ship ASAP in order to meet a hard deadline. The case was the following:

* Have an accordion that is able to expand and contract on click
* Have it be fully accessible, critical importance on keyboard accesibility and semantics
* Make it readable even in older browsers

I had a tough time balancing my options: create a small script from scratch to make this happen, include an external library, OR use **a native HTML element that degrades gracefully and is fully readable in unsupported browsers**.

#### Enter HTML [details](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/details)

After some discussion of time vs. maintainability, accesibility, and other factors, the client decided that it was best to go for the native _details_ element, fully supported in the latest version of most browsers, almost-entirely customizable, and gracefully degrading to a list on older browsers. This was the perfect solution when considering our timeframe and feature requirement. Check out [this pen on CodePen](https://codepen.io/nicolasjengler/pen/qBaMaVJ) that I worked on and helped me decide what approach to pick.

The bare HTML end result looks something like this:

```html
  <details class="warning">
    <summary>Facts and knowledge about COVID-19</summary>
    <p>Some text to be hidden.</p> 
  </details>
  
  <details class="info" open>
    <summary>For the public</summary>
    <p>Social distance, quarantine and isolation</p>
    <p>Hand hygiene, cough etiquette, cleaning and laundry</p>
    <p>When children have acute respiratory tract infections</p>
    <p>Risk groups and their relatives</p>
  </details>
  
  <details class="alert">
    <summary>Facts and knowledge about COVID-19</summary>
    <p>Some text to be hidden.</p> 
  </details>
```

Pretty straightforward, right? You can wrap around that snippet in a tag of your choice to put it in a specific predefined context that's semantic, and both `details` and `summary` elements are customizable via CSS without much complexity.

The W3 Wiki is pretty biased towards Progressive Enhancement, but cases like this introduce a simple question: **is it worth overengineering something that requires a minimal investment of time and LITERALLY zero effort to maintain?**

Lea Verou [wrote an article](https://lea.verou.me/2020/05/todays-javascript-from-an-outsiders-perspective/) that isn't entirely related to this, but presents another question that is worth asking: do we need to keep overcomplicating stuff for developers whenever it isn't needed or crucial? **How do we plan to keep introducing folks to front-end development given how fast the requirements list is growing?**