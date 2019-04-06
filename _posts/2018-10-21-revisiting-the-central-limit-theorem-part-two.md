---
layout: post
title:  "Revisiting the central limit theorem: part two"
date: 2018-10-21 17:00:00 +0800
categories: [tutorials]
tags: [clt, statistics]
asset_path: "/assets/revisiting-the-central-limit-theorem-part-two/"
---

This is the second part of a planned series on the Central Limit Theorem! In this post, we'll talk about the assumptions behind the CLT, which control when it applies.

The first post, which dealt with what the CLT is, can be found [here]({{ site.baseurl }}{% link _posts/2018-10-04-revisiting-the-central-limit-theorem-part-one.md %}). In it, we established that the means of a number of samples taken from a population will be approximately normally distributed, but noted that this is dependent upon several assumptions, which we shall now explore.

<h1>Sufficiently large samples</h1>

The distribution of the values of *each observation* will approximate the population distribution, which is not necessarily normal. Therefore, in the case where each sample contains only one observation, the CLT will clearly not hold since the sample's mean is the same as the value of the single observation in it. 

<img src="{{ page.asset_path }}/varying-sample-sizes.png" width="100%"/> 

As the sample size increases, however, the likelihood of the sample mean being close to the population mean increases, while the influence of single observations on the population sum decreases. These two effects combine to render the (normalised) sample mean distribution more and more normal.

<h1>Independent observations</h1>

Each observation in a sample must be independent, which means that the probability of an outcome must not be affected by previous outcomes. The intuition behind this observation can be sketched as follows: 

As noted above, the samples must be sufficiently large. Consider a sample consisting of five observations, $$x_1, x_2...x_{10}\ \ \ \ $$. In a situation where $$x_2, x_3...x_{10}\ \ \ \ $$ are totally dependent upon $$x_1$$ such that $$x_2 = 2x_1, x_3 = 3x_1...x_{10} = 10x_1\ \ \ \ \ \ \ \ \ \ \ $$, the sample sum is in fact just equal to $$45x_1\ \ $$; in other words, it effectively consists of *only one observation*, and will therefore follow the population distribution of $$x_1\ $$, scaled by a constant. 

In summary, therefore, the greater the interdependence of observations, the smaller the "effective sample size", which in turn decreases the normal character of the sample mean distribution.

<h1>Finite population variance</h1>

The variance of a distribution is the square of its standard deviation, and describes its "spread", or, intuitively, how far an observation will be from the mean:

<p align="center"><img src="{{ page.asset_path }}/variance-comparison.png" width="60%"/></p>

It turns out that some distributions have an infinite or undefined variance. Practically speaking, this means that occasionally, a sample will contain an observation that is so extreme, it changes the mean by a large amount.

Consider this histogram of 1000 means, each calculated from an independent sample of size 250 taken from a population that is [Student's t distributed](https://en.wikipedia.org/wiki/Student's_t-distribution) with 2 degrees of freedom:

<p align="center"><img src="{{ page.asset_path }}/t-distribution.png" width="80%"/></p>

The t-distribution's infinite variance can be observed from the presence of "outlier" sample means arising from single extreme observations.

In the case of the (standard) [Cauchy distribution](https://en.wikipedia.org/wiki/Cauchy_distribution), which has undefined variance (and mean), the distribution of sample means is even more unusual:

<p align="center"><img src="{{ page.asset_path }}/cauchy-distribution.png" width="80%"/></p>

In this case, while the bulk of sample means are near 0, there are a few which deviate greatly, being below -50 or above 50, because they contain single observations that are extremely large in magnitude.

To conclude, where a population does not have finite variance, the samples taken from it might be dominated by infrequent but extreme observations. This changes the means in such a way that they are no longer normally distributed and therefore the CLT does not apply.

<h1>Identically distributed variables</h1>

So far, we have considered only samples taken from the same population. It turns out that each sample can contain observations from different populations. In other words, the random variables being sampled can be distributed in different ways (normal, Student's t, chi-squared) or in the same way, but with different parameters (mean and variance for normal distributions, degrees of freedom for Student's t and chi-squared).

Imagine we have a sample consisting of $$x_1, x_2 ... x_n\ \ \ \ $$.  

If $$x_1, x_2 ... x_n\ \ \ $$ do not all come from the same distribution, then the classical CLT may not apply, which would mean that the sample sum ($$x_1 + x_2 + ... + x_n\ \ \ \ \ \ $$) and mean ($$\frac{x_1 + x_2 + ... + x_n}{n}\ \ \ \ $$) will not be normally distributed.

In practice, the sample sums and means can still be normally distributed if something called [Lindeberg's condition](https://en.wikipedia.org/wiki/Lindeberg%27s_condition) holds. The reason the CLT assumes identical distribution of variables is that, loosely speaking, each random variable should not contribute "too much" to the variance of the sum; just how much is "too much" is determined by Lindeberg's condition.

<h1>Summary</h1>

We have covered four of the CLT's underlying assumptions. Practically speaking, they effectively restrict how much one single observation can influence a sample. This leads to the sample means, in particular, following a roughly predictable pattern centred around the population mean, and this pattern happens to be a normal distribution.

In the next post, we'll discuss how we can actually use the CLT!
