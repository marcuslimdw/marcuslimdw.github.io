---
layout: post
title:  "Stack Overflow thoughts"
date:   2019-05-20 18:00:00 +0800
categories: [thoughts]
tags: [computer-engineering, stack-overflow, education, python, algorithms]
---

I've been on [Stack Overflow](https://stackoverflow.com/users/11015427/gmds?tab=profile) for about three months now, and it's been a pretty good experience. I've always loved teaching, and answering questions there helps me to grow as an educator, a programmer *and* a communicator.

SO is a lifeline for developers around the world. However, it is not like most other sites; the questions there are (dare I say, obsessively) curated by both moderators and experienced users. The good thing about this is that if a question has an accepted answer, it is likely to tell you exactly how to solve your problem.

On the flip side, however, it is *hard* to ask a good question. There is an [art to asking questions](https://stackoverflow.com/help/how-to-ask). and many new users' first questions get downvoted to oblivion or flagged for closing because this question format is so unfamiliar to them. This can make it a rather unwelcoming place for someone who is panicking over a deadline.

The link above is pretty long, but if I had to distil it down to two points, I would choose these two:

1. **Show effort**

Everyone on SO is a volunteer. Saying "I want this" and not even doing a cursory search for solutions or providing a sample of an expected result reflects poorly on an asker, and suggests that they feel *entitled* to a solution, especially if the question relates to a responsibility they have, such as homework or their job. 

2. **Ask narrow questions**

In general, SO is more like a flowchart than an encyclopaedia. Questions should have clear answers that are objectively right. "Why does `1 + 2 * 3` produce 7 and not 9" has such an answer. "How do I implement isotonic regression" doesn't.

Doing these, I think, already sets someone ahead of the pack, which can be extremely valuable when asking questions with fast-moving tags like `javascript` and `python`, and avoid the unfortunate fate of getting hit by mass downvotes *and* close votes simultaneously.

Incidentally, [this](https://stackoverflow.com/questions/55911018/how-to-get-two-lists-that-have-the-most-elements-in-common-in-a-nested-list-in-p/55911813#55911813) is probably my favourite answer. It's not the most upvoted, not by a long shot. It doesn't even actually work.

Nevertheless, it's my favourite because of how I converted the question from being about common sub-elements in lists to a nearest neighbour problem. It links combinatorics, data structures, linear algebra and programming, and it was one of those times that made me just stop and think "Everything really is connected".

It didn't (really) work because `sklearn`'s kd-tree can't be built from a sparse matrix. I'm not really sure if that's a mathematical impossibility (it doesn't seem to be, intuitively...), so if you *can* build a kd-tree efficiently from sparse data, then would this be the fastest way to solve the problem...?
