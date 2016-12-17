---
layout: post
title:  "How to choose the right framework for the job"
date:   2016-05-08 14:12:00 -0300
categories: 'workflow'
---

### Originally on [CodePen](http://codepen.io/nicolasjengler/post/choosing-the-right-framework-for-the-job)

Welp, no one has ever talked about **this**, right? Just kidding...
![A quick Google search on 'choosing web framework'](https://s3-us-west-2.amazonaws.com/s.cdpn.io/86637/Screen_Shot_2016-05-08_at_9.48.22_AM.png)

That being said, I want to talk about the **"how to choose"** rather than diving into the **"choosing"** itself. If that wasn't clear enough, I totally get you, even I am struggling with the idea of what I want to express here, but let me rephrase that and explain it in a different way: most of the articles I've read so far focus on telling the reader that they should choose *X* or *Y* because of *Z*.
Telling the reader that they should choose a certain framework falls back on *absolutism*, which *is inherently bad given the vast and diverse world that web development is*, thus creating many different needs and wants. I believe that an article of this sort should *give the reader the tools so that we*, humble crafters of these small spaces of the internet, *can make a conscius decision accordingly to what we are going to work on*, and the level of complexity it has.

Over the last couple of years that I've worked as a web dev/front-end dev/[*evolving job title here*] I found that following a series of questions helped me when having to make a decision and cut extra features I didn't need. In my case, for example, it usually goes like this:

1. Is extensive **browser support** needed?
2. Are **grids** needed?
3. Will highly customizable or easily achievable **elements** (i.e.: accordions, dropdowns, responsive navbars, etcetera) be used?
4. Are default styles needed in order to put up **quick prototypes** or MVPs?
5. Are **presentational classes** going to be an issue down the road?


Some of these questions can go hand in hand, others may not. Take the first and second items as an example: if I need extensive browser support *AND* I also need grids, Bootstrap or Foundation may be great candidates for the job; but if browser support wasn't an issue, flexbox or the grid layout module can do the job in a jiffy.

The list will definitely vary from developer to developer, but as I said earlier, *this isn't so much about the questions as it is about the __process__*. If you interrogate yourself before you promptly choose a framework, that decision will reflect on your job in the end.
