---
layout: post
title:  "How to fix unexpected gaps when printing flex-based elements in Safari"
date:   2021-02-10 11:00:00 -0300
categories: 'development'
---

### Originally on [Dev.to](https://dev.to/nicolasjengler/how-to-fix-unexpected-gaps-when-printing-flex-based-elements-in-safari-1hk1)

**TL;DR:** if possible, switch all your Flexbox containers for block-level elements. Changing `display: flex` for `display: block` should do the trick as long as you don’t mind stacked content when printing (otherwise you’ll need to tinker around to rearrange your print layout)

I’ve been working for a client for almost 3 years now, his project involves students and teachers that usually *require printing* guides to use whenever *they don’t have an internet connection* to use on their devices.

The platform uses different modular types of content (some of that is video along text, other are images in a grid, and other components are just plain text), which is why whenever building the print styles I decided to remove any extra whitespace along with background images, plain images, iframes etcetera. When testing said print styles, everything seemed in order in major browsers like Chrome, Firefox and Edge, but Safari, for some reason, included weird large gaps in between paragraphs and/or titles and content.

**Theory 1:** hidden images, iframes and grids were causing these gaps. Seemed unlikely given those elements all had the `display` property value set to `none`. After checking out these elements in the devtools, manually removing them from the DOM and printing a PDF, it was obvious this wasn’t the solution.

**Theory 2:** CSS [page breaks](https://css-tricks.com/almanac/properties/p/page-break/) needed to be set in order to get this page printing properly. After setting up `page-break-before`, `page-break-after` and `page-break-inside` property values wherever the issues arised, I tried to print a PDF using Safari but none of these changes gave me the expected results.

**Theory 3:** Similar to what happens with some older JavaScript plugins when combined with Flexbox, there might be sizing issues when printing, causing large computed numbers for widths or heights. After switching all my `display` properties to use a `block` value rather than `flex`, everything showed up as it should in the generated print PDF file using Safari.

I was lucky enough that the print layout for this use case was simple, and stacked content was the inteded result. If you need to print layouts that are a bit more complex, you are most likely going to have to dedicate some time to rearrange your components so they play nicely.