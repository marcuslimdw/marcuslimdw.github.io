---
layout: post
title:  "The birth of toyml"
date:   2019-04-02 18:00:00 +0800
categories: []
tags: [machine-learning, python]
---

I've decided to write my own machine learning library, which I have aptly named `toyml`. While it definitely won't be as efficient as code specifically optimised for production, such as `sklearn`, it should be fast enough to be usable for small-scale tasks, at least. In particular, I want to develop my skills further and write clean, maintainable code. To me, that means:

* Automated testing
* Modular, extensible design
* Logically structured code

This post is rather late, since my first commit was actually in January, and it's already April. I actually didn't do much from end February to now, though, because work/life got more hectic around then. I also realised that my efforts were becoming more and more scattered. For example, I was trying to incorporate a module that would deal solely with data generation for testing purposes. I made a commit a few days ago that removed all of this half-baked stuff, because I want to set a specific, clear goal and stick to it. The bells and whistles can follow after.

A recap of what it can do now:

* Linear regression, using (minibatch) gradient descent. I realise that my code actually doesn't work on more than a few rows at a time if the scale of the features is large, because it runs into integer overflow. I plan to use an analytic solution instead, and keep the gradient descent code for the deep learning module in the future.

* Decision tree classification. This was the first "standard" machine learning model I made, which I'm quite proud of since it was also my first real experiment with functional programming. It actually works, and fairly quickly, too. On a toy dataset of shape `(20000, 10)`, at a maximum depth of 8, it takes ~2.5 seconds (`sklearn` takes 0.5 seconds), achieving a comparable score (99% as accurate or so).

* Markov chain generation. This is a bit more "out there"; I first wrote one the morning of my Meet and Greet, back when I had just graduated from GA, just so I had something to showcase for text generation. This is a more object-oriented approach that prioritises extensibility. but it's really not good for much except as a showcase, in my opinion.

* Basic model selection and interpretation. Common metrics such as recall, precision, mean squared error etc. have been implemented, along with a simple equivalent of `train_test_split` from `sklearn`. There's an approach using higher-level functions that I'd actually like to explore here, but one thing at a time.

What I want to look at next:

* K-nearest-neighbours classification. A naive implementation can be very easily done through brute force, but I want to try using a k-d tree or a ball tree. A stretch goal would be to abstract away the tree fitting process through a common base class, to reduce code repetition in the abovementioned `DecisionTreeClassifier` and these prospective trees.

* Support vector machine classification. I've been reading up, and [this] is an excellent series of posts that explains the mathematical principles behind finding the optimal hyperplane in a very accessible manner.

* Deep learning. A simple feedforward fully connected network with backpropagation would be a good start.

These points are definitely too much to work on all at once, so I'm (hopefully) going to choose one and make sure it's of release quality before moving on to something else. By "release quality", I mean the instantiated model should be able to:

* Fit to data in a reasonable time; say, less than 10 seconds for the abovementioned toy dataset.
* Make predictions almost instantaneously (<0.2 seconds) for the same dataset.
* Within those time constraints, reach an accuracy at least 90% of the industry equivalent (`sklearn` or `keras`)

Given the complexity of SVMs and neural networks, this might be too ambitious...or it might not. If I don't try, I won't know.

On another note, I've been at Eureka for 4 months now. Working in a team definitely has developed my skills of software engineering, and I have learnt to solve problems I didn't even know could crop up back then. I'm learning Scala for real now, since I have to actually write it. It's really different from Python; I'm not sure how I feel about the *language*, but one thing is for sure.

I love the challenge.
