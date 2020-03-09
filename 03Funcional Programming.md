# functools
## Comprehensions
1. list comprehensions

    >>> [x*y for x in range(3) for y in range(3)]
    [0, 0, 0, 0, 1, 2, 0, 2, 4]

2. dict comprehensions

    >>> {x: x ** 2 for x in range(10) if x % 2}
    {1: 1, 3: 9, 9: 81, 5: 25, 7: 49}   

3. set comprehensions

    >>> {x*y for x in range(3) for y in range(3)}
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
    
    
[9.7. itertools — Functions creating iterators for efficient looping — Python 2.7.17 documentation](https://docs.python.org/2/library/itertools.html)

> **Infinite Iterators:**
> 
>   
> 
> Iterator
> 
> Arguments
> 
> Results
> 
> Example
> 
> [`count()`](https://docs.python.org/2/library/itertools.html#itertools.count "itertools.count")
> 
> start, \[step\]
> 
> start, start+step, start+2*step, …
> 
> `count(10) --> 10 11 12 13 14 ...`
> 
> [`cycle()`](https://docs.python.org/2/library/itertools.html#itertools.cycle "itertools.cycle")
> 
> p
> 
> p0, p1, … plast, p0, p1, …
> 
> `cycle('ABCD') --> A B C D A B C D ...`
> 
> [`repeat()`](https://docs.python.org/2/library/itertools.html#itertools.repeat "itertools.repeat")
> 
> elem \[,n\]
> 
> elem, elem, elem, … endlessly or up to n times
> 
> `repeat(10, 3) --> 10 10 10`
> 
> **Iterators terminating on the shortest input sequence:**
> 
>   
> 
> Iterator
> 
> Arguments
> 
> Results
> 
> Example
> 
> [`chain()`](https://docs.python.org/2/library/itertools.html#itertools.chain "itertools.chain")
> 
> p, q, …
> 
> p0, p1, … plast, q0, q1, …
> 
> `chain('ABC', 'DEF') --> A B C D E F`
> 
> [`compress()`](https://docs.python.org/2/library/itertools.html#itertools.compress "itertools.compress")
> 
> data, selectors
> 
> (d\[0\] if s\[0\]), (d\[1\] if s\[1\]), …
> 
> `compress('ABCDEF', [1,0,1,0,1,1]) --> A C E F`
> 
> [`dropwhile()`](https://docs.python.org/2/library/itertools.html#itertools.dropwhile "itertools.dropwhile")
> 
> pred, seq
> 
> seq\[n\], seq\[n+1\], starting when pred fails
> 
> `dropwhile(lambda x: x<5, [1,4,6,4,1]) --> 6 4 1`
> 
> [`groupby()`](https://docs.python.org/2/library/itertools.html#itertools.groupby "itertools.groupby")
> 
> iterable\[, keyfunc\]
> 
> sub-iterators grouped by value of keyfunc(v)
> 
> [`ifilter()`](https://docs.python.org/2/library/itertools.html#itertools.ifilter "itertools.ifilter")
> 
> pred, seq
> 
> elements of seq where pred(elem) is true
> 
> `ifilter(lambda x: x%2, range(10)) --> 1 3 5 7 9`
> 
> [`ifilterfalse()`](https://docs.python.org/2/library/itertools.html#itertools.ifilterfalse "itertools.ifilterfalse")
> 
> pred, seq
> 
> elements of seq where pred(elem) is false
> 
> `ifilterfalse(lambda x: x%2, range(10)) --> 0 2 4 6 8`
> 
> [`islice()`](https://docs.python.org/2/library/itertools.html#itertools.islice "itertools.islice")
> 
> seq, \[start,\] stop \[, step\]
> 
> elements from seq\[start:stop:step\]
> 
> `islice('ABCDEFG', 2, None) --> C D E F G`
> 
> [`imap()`](https://docs.python.org/2/library/itertools.html#itertools.imap "itertools.imap")
> 
> func, p, q, …
> 
> func(p0, q0), func(p1, q1), …
> 
> `imap(pow, (2,3,10), (5,2,3)) --> 32 9 1000`
> 
> [`starmap()`](https://docs.python.org/2/library/itertools.html#itertools.starmap "itertools.starmap")
> 
> func, seq
> 
> func(\*seq\[0\]), func(\*seq\[1\]), …
> 
> `starmap(pow, [(2,5), (3,2), (10,3)]) --> 32 9 1000`
> 
> [`tee()`](https://docs.python.org/2/library/itertools.html#itertools.tee "itertools.tee")
> 
> it, n
> 
> it1, it2, … itn splits one iterator into n
> 
> [`takewhile()`](https://docs.python.org/2/library/itertools.html#itertools.takewhile "itertools.takewhile")
> 
> pred, seq
> 
> seq\[0\], seq\[1\], until pred fails
> 
> `takewhile(lambda x: x<5, [1,4,6,4,1]) --> 1 4`
> 
> [`izip()`](https://docs.python.org/2/library/itertools.html#itertools.izip "itertools.izip")
> 
> p, q, …
> 
> (p\[0\], q\[0\]), (p\[1\], q\[1\]), …
> 
> `izip('ABCD', 'xy') --> Ax By`
> 
> [`izip_longest()`](https://docs.python.org/2/library/itertools.html#itertools.izip_longest "itertools.izip_longest")
> 
> p, q, …
> 
> (p\[0\], q\[0\]), (p\[1\], q\[1\]), …
> 
> `izip_longest('ABCD', 'xy', fillvalue='-') --> Ax By C- D-`
> 
> **Combinatoric generators:**
> 
>   
> 
> Iterator
> 
> Arguments
> 
> Results
> 
> [`product()`](https://docs.python.org/2/library/itertools.html#itertools.product "itertools.product")
> 
> p, q, … \[repeat=1\]
> 
> cartesian product, equivalent to a nested for-loop
> 
> [`permutations()`](https://docs.python.org/2/library/itertools.html#itertools.permutations "itertools.permutations")
> 
> p\[, r\]
> 
> r-length tuples, all possible orderings, no repeated elements
> 
> [`combinations()`](https://docs.python.org/2/library/itertools.html#itertools.combinations "itertools.combinations")
> 
> p, r
> 
> r-length tuples, in sorted order, no repeated elements
> 
> [`combinations_with_replacement()`](https://docs.python.org/2/library/itertools.html#itertools.combinations_with_replacement "itertools.combinations_with_replacement")
> 
> p, r
> 
> r-length tuples, in sorted order, with repeated elements
> 
> `product('ABCD', repeat=2)`
> 
> `AA AB AC AD BA BB BC BD CA CB CC CD DA DB DC DD`
> 
> `permutations('ABCD', 2)`
> 
> `AB AC AD BA BC BD CA CB CD DA DB DC`
> 
> `combinations('ABCD', 2)`
> 
> `AB AC AD BC BD CD`
> 
> `combinations_with_replacement('ABCD', 2)`
> 
> `AA AB AC AD BB BC BD CC CD DD`
