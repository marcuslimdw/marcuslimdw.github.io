---
layout: post
title:  "MapleStory Scroll Efficiency"
subtitle: "A quick Monte Carlo simulation"
date: 2018-08-30 17:00:00 +0800
categories: projects
image_path: "/assets/quick-monte-carlo-simulation/"
---

Recently, someone said to me "Hey, you know data science, right? I have a problem for you..."

It turned out it wasn't a hot button issue like global warming or rampant corruption. My friend just wanted to optimise her MapleStory Mobile character.

Anyway, the gist of the problem, as told to me, is as follows:

You can spend 200 "stones" (a form of currency) on certain equipment. When purchased, the equipment has no upgrades. You can then spend more stones on "scrolls", which are consumable objects. When used on equipment, they may improve it, destroy it, or do nothing. There are three types of scrolls, differing in their costs and the relative probabilities of those three outcomes:

<table style="width:100%">
	<tr>
		<th>Scroll Type</th>
		<th>Improvement Chance</th>
		<th>Destruction Chance</th>
		<th>Fail Chance</th>
		<th>Cost</th>
	</tr>
	<tr>
		<td>1</td>
		<td>10%</td>
		<td>0%</td>
		<td>90%</td>
		<td>20</td>
	</tr>
	<tr>
		<td>2</td>
		<td>30%</td>
		<td>5%</td>
		<td>65%</td>
		<td>40</td>
	</tr>
	<tr>
		<td>3</td>
		<td>50%</td>
		<td>10%</td>
		<td>40%</td>
		<td>70</td>
	</tr>
</table>
<br>
Each piece of equipment can be upgraded a maximum of 5 times (such equipment is called 5-starred), and failing an upgrade does not count towards this 
limit.

The question, then, is <span class="emphasis">which permutation of scrolls will cost the least, on average, if you want 5-starred equipment?</span>

For 10% scrolls, the answer is simply 10% * 5 stars * 20 stones per scroll + 200 stones for the equipment, equalling a total of 1200 stones.

For 30% and 50% scrolls, however, because there is a chance of the equipment breaking, the answer cannot be calculated anywhere near that simply. 

Thus, I decided to use a different approach: the *Monte Carlo method*, which involves using a random number generator to simulate the process of scroll usage. This can be done by writing a simple function:

{% highlight python %}

import numpy as np

def calcScroll(scroll_data, equipment_cost = 200, max_stars = 5):
    success_chance = scroll_data['success']
    break_chance = scroll_data['break']
    scroll_cost = scroll_data['cost']
    n_stars = 0
    scroll_count = 0
    equipment_count = 1
    total_cost = equipment_cost
    
    while n_stars < max_stars:
        scroll_count += 1
        total_cost += scroll_cost
        chance = np.random.rand()
        if chance < success_chance:
            # success
            n_stars += 1
            
        elif chance < success_chance + break_chance:
            # equipment broke...gotta shell out money.
            n_stars = 0
            equipment_count += 1
            total_cost += equipment_cost
            
        else:
            # failure, so nothing happens
            pass
        
    return (equipment_count, scroll_count, total_cost)

{% endhighlight %}

A call to this function will return, for that particular trial, pieces of equipment required (in case of breakage), the number of scrolls used, and the total cost. I chose to run 100,000 simulations per type of scroll, resulting in 300,000 data points. This took about 3 seconds, and gave me more than enough data, with which I was able to run some visualisations.

<img src="{{ page.image_path }}/scrolls-needed.png" width="100%"/>

You might wonder why the minimum value for scrolls is 0. This is because, to get 5 stars, at least 5 scrolls will always be necessary. Therefore, this visualisation tracks instead the number of *extra* scrolls beyond those 5 that are needed per trial.

From this graph, we can see that, in most cases, if you use purely 50% scrolls, you are likely to use the least number of scrolls, followed by using only 30% scrolls and then 10% scrolls.

This is to be expected since, after all, the 50% scrolls have the highest chances of success. However, this is not the only factor affecting total cost. 

Firstly, 30% and 50% scrolls cost more (up to 3.5 times more!) than 10% scrolls. 

Also, because 30% and 50% scrolls have a chance to cause equipment breakage, the cost of replacement equipment must be factored in, and one piece of equipment is equivalent to <span class="emphasis">ten</span> 10% scrolls in terms of cost. Therefore, we have to look at how many pieces of replacement equipment we are likely to need.


<img src="{{ page.image_path }}/equipment-needed.png" width="100%"/>

This shows how many pieces of equipment were purchased for each trial (the results for the 10% scroll are not shown because there is no chance of breakage). In more than half of the trials for both 30% and 50% scrolls, at least one instace of equipment breakage occurred. Imagine having spent over 500 stones on upgrading your equipment: not only have you wasted all the money you spent on scrolls, you will have to fork out even more for replacement equipment. This chance of breakage is what really pushes the average cost of using 30% and 50% scrolls up.

<img src="{{ page.image_path }}/total-cost.png" width="100%"/>

Overall, <span class="emphasis">using 10% scrolls is generally the most cost-effective path to a fully upgraded piece of equipment</span>. As calculated earlier, doing so will cost about 1,200 stones. Using 30% scrolls will cost around 1,350 stones, and using 50% scrolls a little over 1,500 stones.

Of course, this analysis only deals with situations where all the scrolls you use are of one type. It's quite likely that using a higher probability scroll for the first few stars might be even more cost-efficient because the loss from breakage is less severe. If I have time, I'll look into that in a later post!
