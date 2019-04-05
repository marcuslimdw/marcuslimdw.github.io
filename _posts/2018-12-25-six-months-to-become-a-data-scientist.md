---
layout: post
title:  "Six Months To Become A Data Scientist"
subtitle: "Reflections on GA-DSI"
date: 2018-12-25 17:00:00 +0800
categories: personal
image_path: "/assets/six-months-to-become-a-data-scientist/"
---

As a preamble - I would like to finish my posts on the CLT and on classification (I actually have the illustrations ready; it's just a matter of planning and writing), but life has been hectic, what with capstone consultations, starting a new job and just life in general. Someday. Hopefully before ~~the new year~~ Chinese New Year.

<img src="{{ page.image_path }}/progress-bar.png" width="55%" align="right"/>

Anyway, some have asked me about what the Data Science Immersive experience at General Assembly (GA-DSI) is like. Having gone through it first as a student and then as an instructional associate (I find it sounds nicer than "teaching assistant", though it is harder to type), I figured I would make a public post about what I went through and how I felt.

I was not asked by GA to write this, and nobody has reviewed this post. These are my whole, unfiltered thoughts about the course.

The executive summary: if you've never tried statistics/programming before, you should probably take another online course to see if it's for you. It can give you a head start in data science, but you *will* be learning entry-level skills, and you *will* need to put in a lot of work. It's not for the faint-hearted. The people you'll meet and the GA brand are great, though, so capitalise on those strengths!

<h1>What differentiates this from other courses?</h1>

Community and marketing.

By and large, nobody is there who has not made a conscious, committed choice to be there. You will be united by this, and by your common desire to find employment in data science. In both my batch and the following batch (the one I taught), information and encouragement were freely shared. The value of a group of people you can discuss work with, especially when being thrown into the deep end, is inestimable.

Also, GA has pretty good branding (in my opinion, anyway). They will *not* find a job for you, but they will help you along the way. Most of my batchmates and students have already found employment (taking about 1-3 months after graduation). Of course, the quality of the opportunities you get will differ based on your skill, determination and luck, but overall there shouldn't be a problem finding an entry-level job.

<h1>Is this for everyone?</h1>

In a word, no.

There are people who have dropped out, albeit not many. There are those who struggled through with lots of lost sleep. Talent does matter. In general, those who have structured, logical approaches to problem-solving and who prefer quantitative results will do better, but it's all about changing your worldview.

That said, fortunately, there are many online courses on programming, statistics and data science. If you're considering this path and have time to spare, I would suggest you try programming on your own first.

Remember also that data is a wide field and this is only the first step. Though the course is called a data *science* immersive, the road has many branches. 

<h1>What does the course cover?</h1>

If you're reading this, the subtext is probably "Can I get a job?"

Yes, but there's a lot of variability in what a "data analyst" or "data scientist" does, so just looking at the job title might not tell you much about what you will actually do. In general, though, you should be fairly equipped for an entry-level position.

In detail: the focus is on two areas of technical competence: statistics and Python programming. 

The statistics portion is largely foundational: basic concepts such as probability, A/B testing, confidence intervals and sampling methods; the Python part mainly involves two libraries: `pandas` and `scikit-learn`. 

For data analysis and machine learning in a small-scale context using Python, these are indubitably the gold standard. If you will be doing exploration/modelling on smaller datasets, these are, to a fairly large extent, all you will need. Note that work is often done not in scripts, but Jupyter notebooks, which are an interactive tool useful in exploration and experimentation.

<img src="{{ page.image_path }}/what-i-really-do.png"/>

The machine learning part deserves a bit more elaboration; the following topics are covered:

* Shallow supervised learning (both classification and regression)
	* Nearest-neighbour approaches
	* Linear/logistic regression
	* Support vector machines
	* Random forests
	* Gradient boosting
* Unsupervised learning by clustering
* Natural language processing
* Time-series analysis
* A very brief introduction to deep learning (neural networks)

There is not much emphasis on workflow tools, other than `git`, which is a version control system. Git, in conjunction with hosting sites such as GitHub and BitBucket, allows you to automatically keep successive revisions and branches of code so that multiple people can work on different stages of a project at once.

<h1>What <em>doesn't</em> the course cover?</h1>

GA focuses mainly on smaller datasets which one machine can handle. If you are looking for a position in big data, then being able to perform data science in a distributed computing context will be important, and `pandas` and `scikit-learn` are unsuitable. 

In my current position, I need to deal with much larger volumes of data (gigabytes-terabytes); the main tool I use is `PySpark`, which is the Python version of Apache Spark, a framework for distributed computing. Rather than using Jupyter Notebooks, too, there is a need to write scripts and modules, which is not something I did much in DSI. 

Lastly, I had to get familiar with the whole technology workflow stack really quickly - most of these tools were not touched on during DSI. For example, on a daily basis, I use:

<img src="{{ page.image_path }}/what-you-expect.png" width="55%" align="right"/>

* JIRA to track issues that need to be resolved
* Git for version control
* BitBucket as a git repository
* Hadoop, through Hive, to store and access data
* Slack to communicate with team members

Feedback from my students suggested that the course, overall, did not give a very good "big picture" view of how a data science project would actually proceed in the real world.

DSI focuses mainly on the *technical* skills, but understanding the data science workflow and being conversant with the tools that make data science possible are equally important, and will have to be done on your own time.

<h1>What was my experience like?</h1>

Less than a month before I began the course, I was working in a law firm. However, I found that the firm tended to be stuck in traditional ways of doing things, which were, unfortunately, very inefficient. I was, and am, not the kind of person who can go without saying "I'm sure there's a better way to do this."

One day, I decided I wanted to do something where I could work towards a better tomorrow every day, so I quit and signed up for DSI.

Yes, I did not take my own advice on trying out a programming course first. Don't follow my example - I have dabbled in code on and off since I was 10, and I have always been interested in technology.

As mentioned above, the great thing about GA is the camaraderie. Perhaps because there is no direct competition, there is a strong culture of sharing the load. It was hard, but it was also really fun, because every day there was a new problem to solve, and something to learn in the process.

I liked it a lot, which was probably why I went back as an instructional associate. 

<h1>What could be improved?</h1>

Two (three for the current batch) instructional associates are chosen from each batch to help out the next batch, based on technical ability, social skills and some other stuff. When I applied and was chosen, I resolved to improve those things that I found lacking in what I myself went through, in particular:

1. Better preparation for employer's assessments. I devised a technical test to mimic the ones you might be required to do before or during an interview. I also pushed for, and personally conducted, technical interviews.

2. The bigger picture. Where I could, I always discussed how I thought a particular part of the process would fit in an industry setting, and practical paths to take.

3. What underlies abstractions. I believed that it was not enough to know how to use something; I should have a basic understanding of its internal workings, so that if it breaks, I could fix it. I tried to exemplify this attitude by encouraging writing good code and thinking about how, and why, code works.

It was not easy to always keep a few steps ahead of the students; many would ask very perceptive questions. To answer them satisfactorily, I had to have a very firm grasp of the subject material. It was a wonderful opportunity for me to really delve into coding, statistics and machine learning, and inspired the posts that I have made (and have yet to make). Though the future is uncertain, I hope my path leads back to being an educator someday.

Today, reflecting upon what I did, there were definitely things that could have been different, and I shared those with the IAs for DSI-6, to continue the trend of improvement. There were things I only learnt after I began employment that I did not know, and I have tried to list those above. Nevertheless, walking alongside the students of DSI-5 on the journey was an incredibly enriching experience, which I am extremely thankful for. Though there were times of darkness, I think I can say, at the end, that I did my best.