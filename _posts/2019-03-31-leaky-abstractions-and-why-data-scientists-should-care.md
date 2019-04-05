---
layout: post
title:  "Leaky abstractions and why data scientists should care"
date:   2019-03-31 18:00:00 +0800
categories: []
tags: [computer-engineering, animation, data-science]
---

Some people say that data scientists are people who are better at computer engineering than statisticians and better at statistics than computer engineers. How much better at computer engineering one needs to be, though, is a matter of debate. Some are happy with the bare minimum needed to obtain results.

I think that every data scientist should be acquainted with the fundamentals of good software design, software engineering and computer science, and I'd like to bring up the principle of leaky abstractions to explain why.

First, what's an abstraction? In general terms, it's an interface that something or someone presents to you to hide all the gory details that go into getting you what you want.

So, for example, room service is an abstraction. I don't have to know, or care, about all the little details that go into getting me my food, like which supplier the hotel uses, or how to plant wheat, or setting up an order fulfilment system. To me, it's a black box that lets me call, order, wait, pay, eat. The end. As long as I do everything expected of me, I should get what I expect - food.

The problem is, sooner or later, abstractions _leak_. 

In my case, after my order was taken, the hotel neglected to key it into their system, which resulted in me not getting my food. If the room service abstraction was a truly infallible black box, this wouldn't have happened - after all, I called, ordered and waited, and was prepared to pay.

However, one can never ensure that all the implementation details work as they're supposed to 100% of the time. Eventually the implementation fails, with the consequence of that failure _leaking through_ the abstraction to affect the user.

How does this apply to computer engineering and data science? Well, abstractions are everywhere. Let's look at one of the most common objects in data science, the `numpy.ndarray`. It's an object representing an _n_-dimensional fixed-size array, and one of its most important uses is as the fundamental data container for the `pandas` library. Based on what it's supposed to do, the orientation of rows vs columns shouldn't matter, right?

Let's test it with code:

```python
>>> from timeit import timeit
>>> setup = 'import numpy as np; array = np.zeros((10000, 10000))'
>>> add_to_first_row = 'array[0, :] + 1'
>>> add_to_first_col = 'array[:, 0] + 1'
>>> timeit(add_to_first_row, setup, number=100000)
0.5451091610011645
>>> timeit(add_to_first_col, setup, number=100000)
15.08617207599309
```

This snippet creates an `ndarray` of dimension `10000x10000` filled entirely with zeroes. It then times how long it takes to add 1 to all elements in the first row, versus how long it takes to do the same to all elements in the first column. Both ways, it's 10,000 additions. They should therefore, more or less, run similarly from the user's perspective.

However, we can see that `add_to_first_row` takes _30x less time_ to run than `add_to_first_col`. Clearly, the array abstraction is leaking; the way it's laid out in memory and actually implemented does affect the performance of operations on it. 

The reason is that memory is laid out sequentially, like a really long line of 0s and 1s. In this 1-dimensional space, all the data of the first row, `[0, :]` is put together, then all the data of the second row, `[1, :]`, and so on. This means that if you want to overwrite what's kept in a single row, you can write one single large block of memory and be done.

However, this also implies that the elements in the first column, `[0, 0]`, `[1, 0]`, and so on are *not* together. If you want to write to all of them, then, you need to skip a large area of memory after each element. This skipping is the cause of the difference in running time.

An illustration:

<img src="/static/img/memory-layout.gif">

Now, let's try something:

```python
>>> setup = 'import numpy as np; array = np.zeros((10000, 10000), order='F')'
>>> timeit(add_to_first_row, setup, number=100000)
15.24376110999583
>>> timeit(add_to_first_col, setup, number=100000)
0.5439405910001369
```

By adding an additional argument `order='F'` when instantiating the array, we have reversed the situation: now the first-column addition is much faster than the first-row one!

What does `order='F'` do? Per the [numpy docs](), it aligns the array's memory in column-first order, rather than the default row-first order. These results are therefore exactly what we would expect, since now writing to columns can be done in a chunk, while writing to rows requires skipping.

Abstractions are beautiful and convenient - without `numpy`, Python would be more or less unusable for data science - but they will, eventually, leak. This is not an isolated phenomenon; [you should avoid `apply` and `map` in `pandas` whenever you can](https://stackoverflow.com/questions/54432583/when-should-i-ever-want-to-use-pandas-apply-in-my-code), and [`matplotlib`'s 3D plots aren't really 3D](https://stackoverflow.com/questions/13932150/matplotlib-wrong-overlapping-when-plotting-two-3d-surfaces-on-the-same-axes).

Knowing the basics of the abstractions you use is the computing equivalent of being able to change a tyre. A driver is not a mechanic, but understanding why simple and common problems occur is a huge aid to identifying and fixing them, while saving a great deal of time. Often, just poking around in the documentation of libraries you use often and looking at all those default arguments that you never play with can teach you a lot.
