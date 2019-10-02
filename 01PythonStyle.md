# Write beautifully

## Code style
PEP8 PEP20  need to compared here

## Use f-string to format a string
After python 3.6, f-string is suggested to use rather than %-formatting and str.format(). It much more easily readable and faster.
* Multiline f-strings
```python
>>> message = (
...     f"Hi {name}. "
...     "You are a {profession}. "
...     "You were in {affiliation}."
... )

>>> a = 123.456
>>> f'a is {a:8.2f}'
'a is   123.46'
>>> f'a is {a:08.2f}'
'a is 00123.46'
>>> f'a is {a:8.2e}'
'a is 1.23e+02'
>>> f'a is {a:8.2%}'
'a is 12345.60%'
>>> f'a is {a:8.2g}'
'a is  1.2e+02'
```

* Attention

1. Using double quotation marks for the f-string containing the keys, when use single quotation marks for the keys of the dictionary.
```python
>>> comedian = {'name': 'Eric Idle', 'age': 74}
>>> f"The comedian is {comedian['name']}, aged {comedian['age']}."
The comedian is Eric Idle, aged 74.
```

2. In order to show brace, double braces must be used.
```python
>>> f"{{{74}}}"
'{74}'
```

More detail please refer to [f-string.md](./details/f-string.md)

## Flat is better than nested
Nested code quickly becomes hard to understand. Generally, we should refactor when there are three levels of nested loops.

## Sparse is better than dense
Split code logically to make easier to read.

## Errors should never pass silently
Bare or too broad exception catching is bad idea.

## Namespaces are one honking great idea
Namespaces can make code a lot clearer to use. For example, to User object like this way:

    from django.contrib.auth.models import User
    # Use it as :User

But, someone may not know where 'User' come from when they don't see import part. Follows are better:

    from django.contrib.auth import models as auth_models
    # Use it as auth_models.User

## Take care when to compare identity
Python keeps an internal array of integer objects for all integers between -5 and 256, so '256 == 256' equal '256 is 256'. Other conditions are not. 'is' have a better performance in some condition, so **when comparing Python singletons such as True, False and None** using 'is'.

Also, in some condition:

    spam = range(100000)
    eggs = range(100000)

It is better use 'is' to do simple identity check. Because, '==' will compare every item, and 'is' only execute if id(spam) == id(eggs) internally.

## Tools to verify code quality
### Pep8
Check with PEP8 standard
### pyflakes
More intelligent than pep8 and warn some style issues and potential bugs.
### McCabe
It finds out if your code has more complexity than a preconfigured threshold.
### flake8
flake8 combines upper tools and outputs a single report.
### Pylint
pylint is a far more advanced—and in some cases better—code quality checker. The 
power of pylint does come with a few drawbacks, however. Whereas flake8 is a 
really fast, light, and safe quality check, pylint has far more advanced introspection and is much slower for this reason.