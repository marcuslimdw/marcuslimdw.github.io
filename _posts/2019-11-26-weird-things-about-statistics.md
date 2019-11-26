---
layout: post
title:  "Weird things about statistics"
date:   2019-11-26 18:00:00 +0800
categories: [tutorials]
tags: [statistics]
---

Anyone heard of Abraham Wald?

He was a Jewish statistician, born in what was then Austria-Hungary at the turn of the century. As World War II began to rear its dreadful head, he fled to the United States, avoiding persecution.

The most well-known story about him goes something like this:

Planes are expensive to build, and therefore to lose. Armouring them makes them more difficult to lose, but armour, and the fuel required to sustain it, are both expensive. Therefore, the military commissioned a study into how planes could be best and most efficiently armoured. In particular, they looked at where bullet holes were distributed on planes returning from battles.

The next step, obviously, was to put more armour on where there were the most holes.

Wald disagreed. He said, put the armour where the holes *aren't*. The planes that come back have lots of holes in places are not important. The places where there are no holes - that's where the planes that never came back got shot. That's where you need more armour.

This is an example of survivorship bias. We look only at the members of the population that "succeed" in some way, and ignore those who don't, and from that limited sample make claims about the whole, which, as Wald showed, can be utterly, utterly wrong.

After understanding the reason he said that, it makes sense, doesn't it? Yet the reverse conclusion made equally as much sense before. Statistics is a weird discipline.

Another question. Say there's a class of university students. How would one go about finding the average number of children per family, for that class?

Could you go to each student and ask "how many children are there in your family?", total up the responses and divide them by the number of students?

You could, but that would not be right, either, because if there are multiple children from the same family, that counts them multiple times, and families with more children are likely to have multiple children in university, assuming of course that all chldren have equal chances to enter university.

To illustrate: say there are three children in class. One is an only child. The other two come from the same family, which also contains eight other children.

If you asked the above question to each of them and calculated the average, you would get 21, which divided by 3 is 7. However, there are only two families, and the average number of children is simply 11 divided by 2, or 5.5.

I believe this can be considered a type of sampling bias, though I don't think this is the precise term; there is probably something more apposite. However, it is yet another example of how the straightforward approach is the wrong one.

One last example, not really statistics related.

I have four cards, each of which has a number on one side and a colour on the other: a 3, an 8, a red card and a brown card.

Now, I have a rule - cards with even numbers on one side are red on the opposite side.

Which cards must I turn over to test this rule?

The answer is the 8 card and the brown card.

If the 8 card is not red on the other side, that breaks the rule. Similarly, if the brown card has an even number on the other side, that too breaks the rule.

The red card can have anything on the opposite side; the rule works only one way. Similarly, since the 3 is odd, no requirements are imposed on its opposite face.

Was that hard?

What if the context was changed?

I have two people, 13 and 19 years old respectively, drinking unknown drinks. I have two other people, one of whom is drinking milk, and the other beer, of unknown age. 

The rule - only people above 18 can drink beer. Which people do I have to check to make sure that nobody is breaking the rule?

When the exact same problem is put in a social context, it becomes much easier. This test, by the way, is called the [Wason selection task](https://en.wikipedia.org/wiki/Wason_selection_task), and it is over 50 years old.

The crux of this all is that our brains are tremendously powerful things, but also fearfully blind in places we would expect to see clearly. Life, in some ways, is a timeless struggle to see past the heuristics and biases that evolution and society have saddled us with.

(This post would be better with images, but I wrote it in a hurry, before motivation faded.)
