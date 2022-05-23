# Generators
1. pros:
* Generators pause execution completely until the next value is yielded, which makes them completely lazy.
* Generators have no need to save values.
* Generators can have infinite size.

2. cons
* never know how many values are left;
* cannot slice generators
* cannot get specific items without yielding all values before that index
* cannot restart a generator

## creation
1. function using 'yield'
```python
    >>> def generator():
    ...     yield 'this is a generator'
    ...     return 'returning from a generator'
```    
2. Generator comprehensions

    >>> generator = (x ** 2 for x in range(4))

3. class with __next__
The biggest difference between the class and the function-based approach is that you are required to raise a StopIteration explicitly instead of just returning it.
```python
>>> class Count(object):
...     def __init__(self, start=0, step=1, stop=10):
...         self.n = start
...         self.step = step
...         self.stop = stop
...
...     def __iter__(self):
...         return self
...
...     def __next__(self):
...         n = self.n
...         if n > self.stop:
...             raise StopIteration()
...
...         self.n += self.step
...         return n
```

## lasy
```python
>>> def generator():
...     print('Before 1')
...     yield 1
...     print('After 1')
...     print('Before 2')
...     yield 2
...     print('After 2')
...     print('Before 3')
...     yield 3
...     print('After 3')
>>> g = generator()
>>> print('Got %d' % next(g))
Before 1
Got 1
>>> print('Got %d' % next(g))
After 1
Before 2
Got 2
```

## piplines - an effective use of generators
Like linux shell 'ps aux | grep python', we can wrap a list or sequence multiple times with little performance impact use generators.
```python
>>> def cat(filename):
...      for line in open(filename):
...          yield line.rstrip()
...
>>> def grep(sequence, search):
...      for line in sequence:
...           if search in line:
...               yield line
...
>>> def replace(sequence, search, replace):
...     for line in sequence:
...          yield line.replace(search, replace)
...
>>> lines = cat('lines.txt')
>>> spam_lines = grep(lines, 'spam')
>>> bacon_lines = replace(spam_lines, 'spam', 'bacon')
>>> for line in bacon_lines:
...     print(line)
...
bacon
bacon bacon
bacon bacon bacon

# Or the one-line version, fits within 78 characters:
>>> for line in replace(grep(cat('lines.txt'), 'spam'),
...'spam', 'bacon'):
...     print(line)
...
bacon
bacon bacon
bacon bacon bacon
```

## tee - using an output multiple times
One problem of generators is results are usable only once. The tee can split generator to multiple generators.
```python
>>> def spam_and_eggs():
...      yield 'spam'
...      yield 'eggs'
>>> a, b = itertools.tee(spam_and_eggs())
>>> next(a)
'spam'
>>> next(a)
'eggs'
>>> next(b)
'spam'
>>> next(b)
'eggs'
```
## generating from generators
Sometime, we need to return from sub-generators or sequences, using *yield form*  to make code shorter.
```python
# 
>>> def powerset(sequence):
...     for size in range(len(sequence) + 1):
...         for item in itertools.combinations(sequence, size):
...             yield item

# with yield from
>>> def powerset(sequence):
...     for size in range(len(sequence) + 1):
...         yield from itertools.combinations(sequence, size)
```

