---
layout: post
title:  "How to make your Python code clearer with type hints"
date:   2019-07-07 18:00:00 +0800
categories: [tutorials]
tags: [python, type-hinting, computer-engineering]
---

In the course of your work with Python, you might have come across code like this:

```python
def repeat_string(to_repeat: str, repeat_count: int) -> str:
    return to_repeat * repeat_count
```

Note how the function specifies that it expects a `str` for `to_repeat` and an `int` for `repeat_count`, and to return a `str`. This is termed *type hinting*; the actual expressions that represent the types are *type annotations*. By doing so, we can signal to users of our code, as well as third party tools, what data types our functions expect.

Type hinting does not change the behaviour of the function at all; the following function will do exactly the same thing:

```python
def repeat_string_no_hints(to_repeat, repeat_count):
    return to_repeat * repeat_count
```

If you attempt to execute, say, `repeat_string(None, 1)`, it will fail in exactly the same way as `repeat_string_no_hints`: at runtime, with a `TypeError`. What then is the purpose of type hinting?

First, it makes your code easier to read by making your intentions clearer. In general, code is written once and read many times; while writing it, then, it is often worth a little extra effort to focus on readability and simplicity. Type hinting allows a user to understand what kind of objects your code will accept.

Also, a third-party program, such as an IDE (e.g. PyCharm) or a standalone linter (e.g. MyPy), will be able to inspect your code and determine, in most cases, if a function's body is inconsistent with its signature. So, for example, the following function would be highlighted as ill-typed:

```python
def ill_typed_function(left: str, right: str) -> str:
    # It doesn't make sense to multiply strings together!
    return left * right 
```

PyCharm, after inspecting the code, gives the error "Expected type 'int', got type 'str' instead", since it knows that a `str` on the left side of the `*` operator must have an `int` on the right, but the function expects the value on the right to be a `str`, which doesn't make sense. This error won't prevent the code from compiling; it's just a note to me, the programmer, that there is likely something wrong with how I have structured the code, and if I use it the way I have declared it, I will get a runtime error.

Type hinting is not limited to basic types, like `str` and `int`. The `typing` module provides a rich set of objects which you can use for highly descriptive type hinting. For example, say you wanted to signal that your function takes a `list` of `ints` and returns a `dict` that maps `strs` to `ints`; you could do something like this:

```python
from typing import List, Dict

def my_function(my_list: List[int]) -> Dict[str, int]:
    ...  # function body here
```

The above function signature can sometimes be a bit too specific. Recall that Python is a strong proponent of *duck typing*: we usually don't care what exact type an object is, as long as we can use it how we want. Therefore, it's often the case that, when we type-annotate function parameters, we use a type that signals *what we expect the type can do*, rather than *what the type specifically is*. 

For example, say we want some object that we can iterate over, not just a `list` (in other words, something we can do `for thing in obj` with). A `tuple`, a `pandas.Series` or a `numpy.ndarray` would all work for our function. In this case, instead of specifying a `List[int]`, we could go with the more general solution of `typing.Iterable[int]`:

```python
from typing import Iterable, Dict

def my_function_two(my_iterable: Iterable[int]) -> Dict[str, int]:
    ...  # my function body here
```

In the `typing` module, there's also a `Mappable` object, which represents collections that allow you to pass in some object to get another corresponding object. At first sight, that seems a lot like a general case of our `Dict[str, int]`. Why, then, don't we change our return type annotation to `Mappable[str, int]`?

The reason is the general principle that functions should be liberal in what they accept but conservative in what they produce. An extensive discussion of this idea is outside the scope of this article, but you can read more about it [here](https://en.wikipedia.org/wiki/Robustness_principle)). 

Practically speaking, consider this: in your function, you *must* return a specific type. A `Mappable` is a *concept*, which a `dict` is a concrete example of. On one hand, your code clearly cannot return such an abstract concept, since it must return an object of some specific type. On the other, even if it could, it would make it more difficult for others to use your code, since they would have to treat the results as generic `Mappables`, rather than the more specific `dict` type. This would lead to losing information that could be crucial in deciding how to use the returned values.

A full examination of the `typing` module is again beyond the scope of this article, but let's take a quick look at the other things you can do:

* Function type annotations

```python
from typing import Callable, Iterable, List

def filter_strings(predicate: Callable[[str], bool], 
                   strings: Iterable[str]) -> List[str]:
    # returns a `list` of each `str` in `strings` that returns `True` when 
    # `predicate` is called on it, where predicate is a function that takes
    # a `str` and returns a `bool`
    return [string for string in strings if predicate(string)]
```

* Union types (a variable is one of two or more types)

```python
from typing import Union

def take_from_string(string: str, 
                     chars: Union[int, float]) -> str:
    if isinstance(chars, int):
        # gets the first `chars` characters from `string`, up to the length of 
        # `string`
        return string[:chars]

    elif isinstance(chars, float):
        if 0.0 <= chars <= 1.0:
            # treats `chars` as a percentage and gets that percentage of 
            # characters from `string`
            return string[:int(chars * len(string))]  

        else:
            raise ValueError("If 'chars' is a 'float', it must be in the "
                             "range [0.0, 1.0].")

    else:
        raise TypeError("'chars' must be either 'int' or 'float'.")
```

* Specify only some types

```python
from typing import Any

def print_surrounding(surrounding: str, centre: Any):
    print(surrounding)
    print(centre)
    print(surrounding)
```

In my newer projects, such as [spyro](https://github.com/marcuslimdw/spyro), I have type hints for all my implementation code; often, I write the types in the function signature first, to provide a mental roadmap of what I expect the function to do.

That's about it for the how, why and what of type hinting. For completeness, let's explore a little of the background. 

The key PEP to look at is [PEP-484](https://www.python.org/dev/peps/pep-0484/), which goes through the rationale and semantics of type hinting. Long story short, the developers have no intention, now or ever, of making type hinting mandatory, or of turning Python into a statically typed language.

However, statically typed languages do generally prevent a whole class of type-related bugs by allowing certain checks to be done at compile time, instead of runtime. Type hinting is a form of [gradual typing](https://en.wikipedia.org/wiki/Gradual_typing), and helps to provide this compile-time checking benefit where the programmer finds it useful. It's never necessary, but it can be useful. 

I've met people who told me that they moved to Python just so they wouldn't have to specify types, which is understandable, but in my experience, not caring about types and edge cases will come back to haunt you someday. For simple things like throwaway scripts, I don't write type annotations, but I'm sure there are people who do. I think that, overall, it's more about attitude than anything; ultimately, these are all just tools to write good, robust code.
