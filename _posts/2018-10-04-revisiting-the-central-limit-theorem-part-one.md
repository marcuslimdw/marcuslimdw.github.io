---
layout: post
title:  "Revisiting the central limit theorem: part one"
date: 2018-10-03 17:00:00 +0800
categories: [tutorials]
tags: [clt, statistics]
asset_path: "/assets/revisiting-the-central-limit-theorem-part-one/"
---

In school, the Central Limit Theorem (CLT) was brought up briefly, glossed over, and then forgotten. Only when I myself became an educator did I realise I needed to understand it, and its importance, more thoroughly.

First, what _is_ the CLT? [Wikipedia](https://en.wikipedia.org/wiki/Central_limit_theorem) states that it establishes that "in some situations, when independent random variables are added, their properly normalised sum tends toward a normal distribution...even if the original variables themselves are not normally distributed."

That's rather technical, and as it turns out, there are actually several versions of the CLT, each with specific implications. Which you choose depends on the specific context. 

For this series of posts, let's focus on one in particular: let's say you take the heights of a number of adults. The result is one sample. You then repeat this process many times, thereby collecting a large number of samples.

The CLT states that if you calculate each sample's mean and scale them appropriately, the resultant distribution of scaled means will be approximately normal.

This statement depends on some crucial assumptions, which will be dealt with in a later post.

Consider this randomly generated population:

<img src="{{ page.asset_path }}/population-distributions.png" width="100%"/>

This histogram represents the heights of a million adults of each gender, plotted both separately and together. The x-axis is divided into 0.5 cm brackets, and the y-axis shows how many adults fall into each bracket. Each gender's height distribution is normal, but the combined distribution is clearly not. 

What about the samples, then?  Compare a sample of heights taken from the population of males with one taken from the combined distribution (both samples are of size 500):

<img src="{{ page.asset_path }}/sample-distributions.png" width="100%"/>

We can see that the observations in the samples are distributed approximately the same way as the populations they were taken from. In the case of the combined distribution, then, the sample will *not* be normal; that is not what the CLT says.

Instead of looking at the observations, consider only their mean. Imagine that we take an increasing number of samples, calculate the sample means, and plot a histogram of the sample means.

<img src="{{ page.asset_path }}/sample-means.png" width="100%"/>

We can see two things:

1. As more sample means are included, the histogram bins get narrower and narrower.

2. The sample means start to look like they're distributed normally (bell-shaped curve) about the population mean, and the degree of "normality" increases as the number of sample means increases.

What then happens as the number of sample means gets closer to infinity? 

<center><video width="640" height="360" controls> <source src="{{ page.asset_path }}/sample-means-animation.mp4" type="video/mp4"> </video></center><br>

Remember that the area of each bar represents the probability that a sample mean will be within the range represented by that bar, but that range keeps on decreasing. At infinity, the width of any individual bar is 0, and therefore the probability of the sample mean being found to be any one single value is 0. 

Link this to probability density functions: similarly, the probability of a randomly distributed continuous variable being measured to be any one single value is also 0.

This implies that the closer the number of sample means gets to infinity, the more the histogram is able to represent a continuous probability distribution. Which distribution? Refer to point 2 above - as the number of sample means increases, so does the histogram's resemblance to a normal distribution's probability density function.

(It is important to note that a "bell-shaped" curve doesn't always mean a normal distribution, but that's a point for another time; it is possible to mathematically verify that the means *do* tend to a normal distribution, as opposed to another bell-shaped one.)

This is one statement of the CLT: When you take a sample from a population, you can (loosely) treat the sample mean as if it came from a normal distribution. This is useful for several applications, such as deriving confidence intervals and performing hypothesis testing, which will be explored in a later post. However, we should also ask: does this apply for any kind of sample or any kind of population?

The answer is, of course, no. The CLT only applies if certain assumptions are made, but we'll come to that in the next part of this series of posts! 
