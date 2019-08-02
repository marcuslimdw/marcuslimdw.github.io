---
layout: post
title:  "A Python recursion primer"
date:   2019-08-01 18:00:00 +0800
categories: [tutorials]
tags: [python, recursion, animations]
asset_path: "/assets/a-python-recursion-primer/"

---

How would you write a function to calculate the [factorial](https://en.wikipedia.org/wiki/Factorial) of an integer?

You could use a `for` loop:

```python
def factorial_iter(n):
	result = 1
	for i in range(1, n + 1):
		result *= i

	return result
```

This is called an *iterative* method because you go through the numbers in the `range` object one by one, multiplying them by a persistent variable, and eventually return its value once you're done.

An alternative method is to use *recursion*:

```python
def factorial_rec(n):
	if n == 0:
		return 1

	else:
		return factorial_rec(n - 1) * n
```

<img src="{{ page.asset_path }}factorial_stack.gif" width="40%" align="right">

Recursive functions divide their inputs into base and recursive cases. Base cases are those where the function can immediately return a result, such as `n == 0` in `factorial_rec` above. In recursive cases, the function returns a result that depends on calling itself (and that call can also lead to another self call, and so on). Here, every input that is not `n == 0` is a recursive case. 

The key insight of recursion is that every recursive case will eventually lead to a base case, or else throw an error. If this is not true, then there is a possibility of **infinite recursion**, where your function does nothing but call itself over and over. In a lower-level language, this will usually cause a crash, but it generally just leads to a `RecursionError` in Python.

For now, it's sufficient to note that this is actually not true for the `factorial_rec` function above; consider what happens if `n` is negative. We'll never get to the base case since you can't get to 0 from making a negative number more negative! One simple way to handle this situation is to raise a `ValueError` if `n < 0`.

**Problems suited to recursion**

Recursion is particularly helpful when you're dealing with problems which can be deconstructed into smaller sub-problems, *each of which resemble the larger problem in some way*. A great example is JSON parsing; say you have the following JSON (deserialised to a Python `dict`) and you want to find the number of `None` values:

```python
json_data = {'response': {'result': 'success',
                          'header': None,
                          'data': [{'id': None, 'value': 6},
                                   {'id': 1483762, 'value': 0},
                                   {'id': None, 'value': None}]},
             'metadata': {'datetime': {'QXXWR': None, 'KBOHT': None, 'ZMPCM': 29478887},
                          'location': None,
                          'call_ids': [None, 87736, None, None, 3586]},
                          'version': None}
```

We can describe our algorithm very simply by first realising that we can conceptually divide the elements in this JSON into two types: collections and individual objects. For individual objects, we just need to check if they are `None` or not, so a simple comparison will suffice. 

For collections, on the other hand, we need to perform this check on everything they contain, including any sub-collections. This is a perfect fit for recursion; since the whole `dict` is itself a collection. We have two kinds of collections: `dicts` and `lists`. For `dicts`, we need to check the values, and for `lists`, the elements; we can therefore recursively call `find_none_rec` for each object we need to check. 

In code, it would look something like this:

```python
def find_none_rec(json):
    if isinstance(json, dict):
        return sum(find_none_rec(value) for value in json.values())
    
    elif isinstance(json, list):
        return sum(find_none_rec(element) for element in json)
    
    else:
        return int(json is None)  # Not strictly necessary, but clearer.
```

Calling `find_none_rec(json_data)` returns `11`, as expected.

Since [iteration and recursion are equivalent](https://stackoverflow.com/questions/931762/can-every-recursion-be-converted-into-iteration/1662489#1662489), this can also be expressed iteratively:

```python
def find_none_iter(json):
    to_check = list(json.values())
    count = 0
    while to_check:
        element = to_check.pop()
        if isinstance(element, dict):
            to_check.extend(element.values())

        elif isinstance(element, list):
            to_check.extend(element)

        else:
            count += int(element is None)

    return count
```

In this case, I find the recursive solution much simpler and clearer, and the iterative approach is really just explicitly using a `list` to simulate recursive calls anyway..

**Reasons not to use recursion**

Recursion is (relatively) inefficient in Python. If you execute `factorial_iter(1000)`, you should get an answer. If you're using default Python interpreter settings, trying to call `factorial_rec(1000)`, on the other hand, will result in a `RecursionError`.

<img src="{{ page.asset_path }}factorial_stack_overflow.gif" width="40%" align="right">

This is because each function call requires memory to store things like variables and where the function was called from. This memory is taken from a structure called the *stack*, and only released back to it when the function returns.

Each recursive call therefore results in another allocation of memory, but no deallocation will be performed *until a base case is reached*. A deep recursion can take all of the stack's memory; Python detects when this is close to happening and stops code execution.

While you can increase the limit with `sys.setrecursionlimit`, this generally isn't a good idea, because actually hitting the end of the stack will cause your operating system to take note and crash the Python interpreter itself, probably hurling all your session data into the void.

**What about other languages?**

Languages in which recursion is a prominent feature get around this by, among other things, [tail call optimisation](https://stackoverflow.com/questions/310974/what-is-tail-call-optimization/310980#310980). Briefly, where it makes sense to do so, the recursive calls are transformed into the equivalent of a `for` loop. This allows you to express your algorithm in a manner more idiomatic for the language, while not sacrificing the efficiency of low-level iteration.

Python doesn't implement this optimisation [for several reasons](http://neopythonic.blogspot.com/2009/04/tail-recursion-elimination.html). Given that it's largely an imperative language, and it does have very nice stack traces, I'm quite inclined to agree with this decision.

Overarll, while iteration and recursion are technically equivalent, which is appropriate for a given situation will depend on the nature of the problem, as well as the tools you choose to solve it. Since they generally require rather different ways of thinking, practice with both tends to make one a better programmer; broadening horizons and all that.

Lastly, if you're looking for another explanation of recursion, I would definitely take a look at [this post]({{ site.baseurl }}{% link _posts/2019-08-01-a-python-recursion-primer.md %})...
