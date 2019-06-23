---
layout: post
title:  "Spyro, toyml and object-oriented C"
date:   2019-06-22 18:00:00 +0800
categories: [thoughts, projects]
tags: [computer-engineering, machine-learning, python, c]
---

A while back, I began thinking - should I continue with `toyml`, or should I do something else? Realistically speaking, how many people, whether they're employers, enthusiasts, hobbyists...will look at my blog posts, or my code on GitHub? Probably not many at all.

I'm not sure if that sounds a little bitter. It does make me sad that there are things I've made that I think are impressive, but that, in all likelihood, nobody will ever see.

I guess what keeps me going is that even if it doesn't really make a practical difference, I love learning and experimenting.

Anyway, `spyro`, which you can find [here](https://github.com/marcuslimdw/spyro)...

The biggest problem I have is with feature creep, which comes in many forms, mostly involving implementing something that...

* I don't need yet
* I probably won't need ever, because it's cool
* creates a gross imbalance in functionality between parts of a project
* doesn't have tests, just like half of the project

So, for `spyro` 1.0, I created an Excel spreadsheet with specific release criteria: a high-level description of what the product should be able to do, and module-level criteria. Tasks were marked as blank (not done), in progress (implementation and tests not both complete), or done.

Along the way, I made a conscious effort not to *add* tasks (maybe only one), and in fact, dropped a few, when I realised that my original estimates were contributing to the aforementioned feature creep.

It was hard, but I think it turned out well, because I had a clear goal to work towards. I'm basically a data scientist, data engineer and project manager all in one. Startup mentality, right?

The next step is to prepare the release criteria for v0.2. Hopefully, sometime in the future, I'll be able to deploy a pipeline on a "solved" dataset and get good results.

Other than that, I've been trying to implement a [simple object-oriented framework in C](https://github.com/marcuslimdw/object-oriented-c). It's been difficult, since this is basically the first time I'm writing C, and it is very different from all the high-level languages I'm used to, but it's interesting for sure.

I also submitted my entry for the Grab AI for SEA challenge. If I do well, maybe I'll make a reflection post. Hope I do!
