# functools
## Comprehensions
1. list comprehensions

     [x*y for x in range(3) for y in range(3)]
    [0, 0, 0, 0, 1, 2, 0, 2, 4]

2. dict comprehensions

     {x: x ** 2 for x in range(10) if x % 2}
    {1: 1, 3: 9, 9: 81, 5: 25, 7: 49}   

3. set comprehensions

     {x*y for x in range(3) for y in range(3)}
    {0, 1, 2, 4}

## partial - no need to repeat all arguments every time
It used to set default value for arguments in case to repeat arguments.
```python
>>> import functools
>>> import heapq
>>> heap = []
>>> heapq.heappush(heap, 1)
>>> heapq.heappush(heap, 3)

>>> heap = []
>>> push = functools.partial(heapq.heappush, heap)
>>> push(1) # same as heappq.heappush(heap, 1)
>>> push(3)
```

# itertools
Default, itertools functions return a iterable object.
## accumulate - reduce with intermediate results
It is similar to reduce. Different is that accumulate returns the immediate results.
```python
>>> months = [10, 8, 5, 7, 12, 10, 5, 8, 15, 3, 4, 2]
>>> list(itertools.accumulate(months, operator.add))
[10, 18, 23, 30, 42, 52, 57, 65, 80, 83, 87, 89]
```
Default, it use add operator.

## chain - combining multiple results
It combines the results multiple iterators.
```python
>>> a = range(3)
>>> b = range(5)
>>> list(itertools.chain(a, b))
[0, 1, 2, 0, 1, 2, 3, 4]
```

## combinations - combinatorics in Python
It used to create the all possible combinations of the given items of a given length.
```python
# create all possible two elements' combinations of 0,1,2 
>>> list(itertools.combinations(range(3), 2))
[(0, 1), (0, 2), (1, 2)]

# take care of repetition of elements condition
>>> list(itertools.combinations_with_replacement(range(3), 2))
[(0, 0), (0, 1), (0, 2), (1, 1), (1, 2), (2, 2)]
```

## permutations - combinations where the order matters
The permutations is similar to combination. Only different is permutations consider the order.

    >>> list(itertools.permutations(range(3), 2))
    [(0, 1), (0, 2), (1, 0), (1, 2), (2, 0), (2, 1)]

## compress - selecting items using a list of Booleans
It applies a Boolean filter to your iterable, making it return only the ones you actually need. The most important thing to note here is that it's all executed lazily and that compress will stop if either the data or the selectors collection is exhausted.

    >>> list(itertools.compress(range(1000), [0, 1, 1, 1, 0, 1]))
    [1, 2, 3, 5]

## dropwhile/takewhile - selecting items using a function
The dropwhile function will drop all results until a given predicate evaluates to true.

    >>> list(itertools.dropwhile(lambda x: x <= 3, [1, 3, 5, 4, 2]))
    [5, 4, 2]

The takewhile function is the reverse of this. It will simply return all rows until the predicate turns false.

    >>> list(itertools.takewhile(lambda x: x <= 3, [1, 3, 5, 4, 2]))
    [1, 3]

## count - infinite range with decimal steps
It is similar to range. Different is that it is infinite and floating-point numbers can be use. Two optional parameters: step and start. Before using it, how to stop should be considered.
```python
>>> for a, b in zip(range(5, 10), itertools.count(5, 0.5)):
... a, b
(5, 5)
(6, 5.5)
(7, 6.0)
(8, 6.5)
(9, 7.0)
```

## groupby - grouping your sorted iterable
Two things should be take attention.

1. The input needs to be sorted by the group parameter. Otherwise, it will be added as a separate group (you can see this result from first example).

2. The results are available for use only once (see second example).

```python
>>> items = [('a', 1), ('b', 0), ('b', 2), ('a', 2), ('c', 3)]
>>> groups = dict()

#first example
>>> for group, items in itertools.groupby(items, lambda x: x[0]):
...     groups[group] = items  #if results want to be used in other place, 
                               #here should be 'groups[group] = list(items)'
...     print('%s: %s' % (group, [v for k, v in items]))
a: [1]
b: [0, 2]
a: [2]
c: [3]

#second example
>>> for group, items in sorted(groups.items()):
...     print('%s: %s' % (group, [v for k, v in items]))
a: []
b: []
c: []
```

## islice - slicing any iterable
islice used to slice generators

    >>> list(itertools.islice(itertools.count(), 2, 7))
    [2, 3, 4, 5, 6]
    
   
