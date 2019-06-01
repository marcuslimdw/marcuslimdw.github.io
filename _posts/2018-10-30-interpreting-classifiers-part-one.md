---
layout: post
title:  "Classifier interpretation: part one"
date: 2018-10-30 17:00:00 +0800
categories: [tutorials]
tags: [classification, machine-learning, statistics]
asset_path: "/assets/classifier-interpretation-part-one/"
---

Classification refers to dividing observations into a number of categories, and is very common in machine learning. For example, based on a patient's hormone levels, it is possible to test whether she is pregnant; in other words, patients can be put into either of the "Pregnant" and "Not Pregnant" classes. The method of performing this separation is termed a classifier.

Classifiers can be very useful; imagine a good and reliable test for cancer. However, "good" and "reliable" may mean different things to different people. There is therefore a need for an *objective* way to measure the performance of a classifier, and that is where classification metrics come in. Three common metrics are accuracy, recall and precision, which are calculated as follows:

<div>
$$\begin{align}
Accuracy &= \frac{correctly\ predicted\ observations}{total\ observations} \\ \\ 
Recall &= \frac{correct\ predictions\ for\ a\ class}{total\ observations\ in\ that\ class} \\ \\
Precision &= \frac{correct\ predictions\ for\ a\ class}{total\ predictions\ for\ that\ class}

\end{align}$$
</div>

<img src="{{ page.asset_path }}/drunk-sober-comparison.png" width="45%" align="right"/>

If these definitions don't really make sense right now, don't worry; we'll put them in context later. 

Now, imagine there was a New Year's Eve party and 500 people attended. 450 drank enough to be considered drunk, leaving 50 who were sober. We can draw a bar graph to represent this information.

It turns out that everyone at that party was underage, and underage drunkenness is an offence in the city our example takes place in. When the police got wind of the goings-on and turned up, they promptly tested every single partygoer with their breathalysers to determine if they were drunk. Those who tested positive would be arrested.

Unfortunately, breathalysers are not perfect. Due to differences in body chemistry and equipment error, 100 of the *actually* drunk individuals tested as sober, leaving only 350 to be caught, and 5 of the sober ones got a "drunk" reading. This result means that we now have four classes of attendees:

1. drunk, and tested as drunk (350 people),
2. drunk, but tested as sober (100 people),
3. sober, and tested as sober (45 people), and
4. sober, but tested as drunk (5 people).

The relationship between what our tests predict and what is actually the case is usually not illustrated with a bar graph, as used above. A much more common tool is one that is simultaneously ubiquitous, useful and aptly named: the **confusion matrix**.
	
<img src="{{ page.asset_path }}/confusion-matrix-example.png" width="50%" align="right"/>

In a confusion matrix, the rows represent the various possibilities for the actual classes, which we call the *ground truth* (the blue cells). A partygoer will be placed into either of the rows, depending on whether they are actually drunk or sober.

The columns, on the other hand, represent the possibilities for test outcomes, or, more generally, the *predictions* (the red cells). Similarly, a person can be assigned to either of the columns, depending on whether the breathalyser test assesses them to be drunk or sober. By looking at where a particular row and column intersect, we can find out how many people had a certain status and tested as a certain status. 

So, for example, the green cell in the "actually drunk" row and the "tested drunk" column contains the number of partygoers who were actually drunk and whom the breathalyser test also said was drunk. Checking above, we see that the count of 350 in the confusion matrix is accurate.

Confusion matrices are a common way to present the results of classification because they contain all the possible combinations of ground truth classes and prediction classes.	However, sometimes we just want a single number that can describe how good our classification process is, such as the metrics listed above: accuracy, precision and recall.

Accuracy is probably the simplest; intuitively, it measures *how often classification was performed correctly*. Therefore, the breathalyser is accurate where it correctly identifies a drunk person as drunk or a sober person as sober, and inaccurate otherwise. 
<img src="{{ page.asset_path }}/confusion-matrix-accuracy-breathalyser.png" width="50%" align="right"/>

In other words, we can calculate accuracy by adding up the counts in the "correct" (blue) cells and dividing them by the total (red + blue). Putting in the numbers, we have:

<div>
$$\begin{align}
Accuracy &= \frac{correctly\ predicted\ observations}{total\ observations} \\
&= \frac{(45 + 350)}{(45 + 5 + 100 + 350)} \\
&= \frac{395}{500} \\
&= 79\%
\end{align}$$
</div>

<br>
We can see that the breathalyser test's classifications are right 79% of the time; we say it has 79% accuracy, which seems like a pretty good score. This raises a question: is high accuracy always desirable?

The answer is that it depends on the problem you are trying to solve. Imagine now that instead of using a breathalyser, the police are lazy and just say that every single person at the party is drunk, *including the 50 sober ones*. This is what the confusion matrix would look like.
<img src="{{ page.asset_path }}/confusion-matrix-accuracy-police.png" width="50%" align="right"/>

The current accuracy is $\frac{450}{500}\ $, or 90%. Remember that 145 people were sober under the breathalyser test. 100 were actually drunk and are now classified correctly, but 45 were sober and are now classified wrongly, leaving 55 more people identified correctly, or a 11 percentage point accuracy increase.

Note that the number of drunk people is 9 times the number of sober people. In machine learning, this is termed an "imbalanced class problem", where one class - the "minority class" - has much fewer members than the other - the "majority class".

When this happens, accuracy becomes less useful as a metric because we can just predict that all observations are part of the majority class, as the lazy police have, and get a high accuracy score. 

However, this obscures the fact that we will never be able to tell if someone is actually sober (part of the minority class), and therefore our classifier is practically useless, since all sober individuals will be identified and arrested unfairly.

A naïve police chief who uses only accuracy as a metric will mistakenly conclude that the police are doing well in their quest to stamp out underage drinking. To give a more representative view of how the breathalyser stacks up against the lazy police, we need to look at *precision* and *recall*.

Unlike accuracy, which is one score calculated over all the data, precision and recall are metrics assessing performance for individual classes. Let's go back to the confusion matrix for the breathalyser test and look at the majority class - drunk people. Focus first on the green cells.

<img src="{{ page.asset_path }}/confusion-matrix-recall-breathalyser.png" width="50%" align="left"/>
<img src="{{ page.asset_path }}/confusion-matrix-recall-police.png" width="50%" align="right"/>

The recall metric measures how many of those actually in a class were predicted to be in that class; the higher it is, the more "complete" the prediction. Applying the formula given at the start to the "drunk" class, we have:

<div>
$$\begin{align}
Recall &= \frac{correct\ predictions\ for\ a\ class}{total\ observations\ in\ that\ class} \\ \\
&= \frac{Number\ of\ people\ who\ tested\ drunk}{Number\ of\ actually\ drunk\ people}
\end{align}$$
</div>

This gives a recall of $\frac{350}{450} \approx 0.78\ \ \ \ $ for the breathalyser test and of $\frac{450}{450} = 1\ \ \ $ for the lazy police.

<img src="{{ page.asset_path }}/confusion-matrix-precision-breathalyser.png" width="50%" align="left"/>
<img src="{{ page.asset_path }}/confusion-matrix-precision-police.png" width="50%" align="right"/>

Precision, on the other hand, looks at what proportion of those who were predicted to be in a class were actually in that class. It is a measure of the "purity" of predictions. Similarly, applying the formula above to the "drunk" class:

<div>
$$\begin{align}
Precision &= \frac{correct\ predictions\ for\ a\ class}{total\ predictions\ for\ that\ class} \\ \\
&= \frac{Number\ of\ drunk\ people\ who\ tested\ drunk}{Number\ of\ people\ who\ tested\ drunk}
\end{align}$$
</div>

This results in a precision of $\frac{350}{355} \approx 0.99\ \ \ $ for the breathalyser test and of $\frac{450}{500} = 0.9\ \ \ $ for the lazy police.

We can see that while the lazy police have slightly higher recall, the breathalyser test has much higher precision. In context, this means that the lazy police identified a bigger proportion of drunk individuals, but the breathalyser test has a substantially lower rate of false identification. Remember that in the case of the lazy police, all 50 sober individuals would have been wrongly arrested, but for the breathalyser, only 5 people would suffer that fate.

What about the minority class - sober people (the beige cells above)? Using the same formula, we can calculate the precision and recall for both classifiers, which will give us two more scores each:

<img src="{{ page.asset_path }}/confusion-matrix-precision-recall-breathalyser.png" width="50%" align="left"/>
<img src="{{ page.asset_path }}/confusion-matrix-precision-recall-police.png" width="50%" align="right"/>

Incidentally, note that the lazy police's precision for the sober class is undefined, because to calculate precision we divide by the number of people who tested as sober, which is 0, and division by 0 is undefined.

From this, we can see that the lazy police have better recall for drunk people, but zero recall for sober people. That means that they have totally failed to identify any sober individuals, which is unacceptable in this context - all those innocent people would be arrested for no reason. Because most of the people there were in fact drunk, just by identifying a person as drunk no matter what, the lazy police managed to obtain a high accuracy score, and the flaws in their approach were only revealed by considering precision and recall.

In general, in a situation of class imbalance, therefore, it is usually important to look not only at accuracy, but also the recall and precision for the various classes. In particular, in some cases one class is much more important than another. Consider cancer: the majority class (healthy) is much larger than the minority class (cancer-positive). 

For the minority class, if precision is low, all that happens is that those who are initially test positive will go for further, more powerful tests, which will show that they are truly healthy. Conversely, if recall is low, many people who in fact have cancer will be identified as healthy, with catastrophic results. Here, what matters is the recall of the minority class.

To recap, we've covered accuracy, precision and recall, and concluded that we need to look at a problem in detail to understand which metrics are applicable. A future post will go into more advanced concepts, such as the f1-score, ROC curves and precision-recall curves, as well as approach precision and recall in terms of true and false positives and negatives. I hope this was helpful for you, and thank you for reading!
