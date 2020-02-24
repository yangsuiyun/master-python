# List
*append*, *get*, *set* and *len* take O(1) time, *remove*, *insert*, *max*, *min* and *in* have O(n) time complexity.
When try to delete some items in a list, it would be better use List comprehension and filter to do that.
```python
>>> primes = set((1, 2, 3, 5, 7))
# Classic solution
>>> items = list(range(10))
>>> for prime in primes:
...     items.remove(prime)
>>> items
[0, 4, 6, 8, 9]
# List comprehension
>>> items = list(range(10))
>>> [item for item in items if item not in primes]
[0, 4, 6, 8, 9]
# Filter
>>> items = list(range(10))
>>> list(filter(lambda item: item not in primes, items))
[0, 4, 6, 8, 9]
```

# dict - unsorted but a fast map of items
The way a dict works is by converting the key to a hash using the
hash function (calls the __hash__ function of an object) and storing it in a hash table. There may be a hash collision(two or more keys has a same hash value) and runtime can actually increase to O(n) in the worst case where all keys have the same hash.

Another, it won't actually resize the dictionary in memory yet when deleting items form a dictionary. The result is that both copying and iterating the entire dictionary take O(max) time. Only way to work around this issue is by recreating the dictionary.

# set 
Expression | Output | Explanation
---------- | ------ | -----------
spam & eggs | s | Every item in both
spam \| eggs | aegmps | Every item in either or both
spam ^ eggs | aegmp | Every item in either but not in both
spam - eggs | amp | Every item in first but not in the latter
spam > sp |True | True if every item in the latter is in the first

# tuple - the immutable list
They are hashable, they can use as a key in a dict.

    a = {(1,2,3): 'spam'}
Another useful feature is to unpack and pack objects with a variable number of items:
```python
# Unpack with variable length objects which actually assigns as a list, not a tuple
>>> spam, *eggs = 1, 2, 3, 4
>>> spam
1
>>> eggs
[2, 3, 4]
# Which can be unpacked as well of course
>>> a, b, c = eggs
>>> c
4
# This works for ranges as well
>>> spam, *eggs = range(10)
>>> spam
0
>>> eggs
[1, 2, 3, 4, 5, 6, 7, 8, 9]
# Which works both ways
>>> a
2
>>> a, b, *c = a, *eggs
>>> a, b
(2, 1)
>>> c
[2, 3, 4, 5, 6, 7, 8, 9]
```

# ChainMap
 A ChainMap is an updatable view over multiple dicts, and it behaves just like a normal dict. Classical use case as follows:
 ## search through multiple dictionnaries at once
 ChainMap can sotre multiple dictionnaries by reference. Multiple dictionnaries can be 'get' and 'pop' at once. And when sub dictionary change, ChainMap also changed.
 ```python
>>> from collections import ChainMap
>>> inventory = ChainMap(toys, computers, clothing)
>>> inventory.get('Mario Bros.')
None
>>> inventory.pop('Blocks')
200
>>> toys['Nintendo'] = 200
>>> inventory['Nintendo']
200
```
## It maintains a chain of defaults
Configuration of command lines can from: command line arguments, environment variables, local files, etc. And there is a notion of priority of these sources. ChinaMap always get a value of key from left sub-dictionary to right sub-dictionary.
```python
# cli.py
import argparse
import os
from collections import ChainMap

defaults = {'debug': False}

parser = argparse.ArgumentParser()
parser.add_argument('--debug')
args = parser.parse_args()
cli_args = {key: value for key, value in vars(args).items() if value}

config = ChainMap(cli_args, os.environ, defaults)

print(config.get('debug'))
```
cli.py will first get a configuration from cli_args and if failed then from os.environ, it will get from defaults last if above two don't contain the configuration.

## two methods of ChainMap
1. new_child

add a new dictionary in the begging of the old ChainMap

    chain = collections.ChainMap(dict1, dict2)
    chain1 = chain.new_child(dict3)

2. reversed

Reversing the ChainMap.

    chain1.maps = reversed(chain1.maps)

# Counter - keeping track of the most occurring elements
It used to count each elements of a list, a string, or a tuple. 
## create
1. define with tracked data

```python
from collection import Counter
obj = Counter('aabbbcc')
```
2. define with a dict, it's result of counting

    obj = Counter({'a':2, 'b':3, 'c':2})

Above two are same.
## Methods
1. elements

get all elements of original data.

    list(Counter('aab').elements())
    #output:
    ['a', 'a', 'b']

2. most_common(n)

get the top n frequent elements.

    obj = collections.Counter('aabbbcccc')
    print(obj.most_common(2))

    #Output：[('c', 4), ('b', 3)]

3. update

add element to count

4. subtract

delete elements from original data

# deque – the double ended queue
Internally, deque is created as a double linked list, which means that every item points to the next and the previous item. It usually used to implement stack, message queue. It can be used as a circular queue with maxlen parameter. maxlen can be used to keep the last n status messages or something similar. 

    deque([iterable], [maxlen])

```python
>>> a = deque([1,2,3], maxlen=3)
>>> a.append(4)
>>> a
deque([2,3,4])
>>> a.pop()
deque([2,3])
>>> a.popleft()
2
```

# defaultdict - dictionary with default value
defaultdict receive str, list, set or int as parameter. It set a default value instead raise a keyerror when the key is not exist.

```python
>>> counter = collections.defaultdict(int)
>>> counter['spam'] += 5
>>> counter
defaultdict(<class 'int'>, {'spam': 5})
```
defaultdict is a good method to implement tree.
```python
>>> def tree():
...     return collections.defaultdict(tree)
# Build the tree:
>>> taxonomy = tree()
>>> reptilia = taxonomy['Chordata']['Vertebrata']['Reptilia']
>>> reptilia['Squamata']['Serpentes']['Pythonidae'] = [
...                         'Liasis', 'Morelia', 'Python']
# The actual contents of the tree
>>> print(json.dumps(taxonomy, indent=4))
{
"Chordata": {
    "Vertebrata": {
        "Reptilia": {
            "Squamata": {
                "Serpentes": {
                    "Pythonidae": [
                        "Liasis",
                        "Morelia",
                        "Python"
                        ]
                    }
                }
            }
        }
    }
}
# The path we wish to get
>>> path = 'Chordata.Vertebrata.Reptilia.Squamata.Serpentes'
# Split the path for easier access
>>> path = path.split('.')
# Now fetch the path using reduce to recursively fetch the items
>>> family = functools.reduce(lambda a, b: a[b], path, taxonomy)
>>> family.items()
dict_items([('Pythonidae', ['Liasis', 'Morelia', 'Python'])])
# The path we wish to get
>>> path = 'Chordata.Vertebrata.Reptilia.Squamata'.split('.')
>>> suborder = functools.reduce(lambda a, b: a[b], path, taxonomy)
>>> suborder.keys()
dict_keys(['Serpentes'])
```

# namedtuple - tuples with field names

```python
>>> Point = collections.namedtuple('Point', ['x', 'y', 'z'])
>>> point_a = Point(1, 2, 3)
>>> point_a
Point(x=1, y=2, z=3)
>>> point_b = Point(x=4, z=5, y=6)
>>> point_b
Point(x=4, y=6, z=5)
>>> x, y, z = point_a
```

# enum - group of constants
```python
>>> import enum
>>> class Color(enum.Enum):
...     red = 1
...     green = 2
...     blue = 3

>>> Color.red.name
'red'
>>> Color.red.value
1
>>> isinstance(Color.red, Color)
True
>>> Color.red is Color['red']
True
>>> Color.red is Color(1)
True

#Enum is iterable
>>> for color in Color:
...     color
```

You can make value comparisons work through inheritance of specific types,
and this works for every type—not just integers but (your own) custom types as well.
```python
>>> class Spam(str, enum.Enum):
...     EGGS = 'eggs'
>>> Spam.EGGS == 'eggs'
True
```

# OrderedDict - a dictionary where the insertion order matters
OrderedDict will return your keys by the order of insertion. Internally, OrderedDict uses a normal dict for key/value storage, and in addition to that, it uses a doubly linked list to keep track of the next/previous items. To keep track of the reverse relation (from the doubly linked list back to the keys), there is an extra dict stored internally.

The system is structured in such a way that set and get are really fast O(1) , but the object is still a lot heavier (double or more memory usage) when compared to a regular dict .

# heapq - the ordered list
It is simply a bunch of methods for treating a regular list as a heap. It is sorted but not the way you expect it to be. If you view the heap as a tree, it becomes much more obvious:
```python
>>> import heapq
>>> heap = [1, 3, 5, 7, 2, 4, 3]
>>> heapq.heapify(heap)
>>> heap
[1, 2, 3, 7, 3, 4, 5]
#
#        1
#    2       3
#  7   3   4   5
```
The smallest number is always at the top and the biggest numbers are always at the
bottom of the tree. It's easy to find the smallest number.

# bisect - the sorted list
Like heapq, it works on a standard list and expects that list to always be sorted, instead of creating a special data structure. A big difference between heapq and bisect is that adding/removing items with the heapq module is very light whereas finding items is really light with the bisect module. If your primary purpose is **searching**, then bisect should be your choice.

Adding items to the list using the bisect algorithm can be very slow because an insert on a list
takes O(n) . Effectively, creating a sorted list using bisect takes O(n*n) , which is quite
slow, especially because creating the same sorted list using heapq or sorted takes O(n * log(n)) instead.
```python
>>> import bisect
Using the regular sort:
>>> sorted_list = []
>>> sorted_list.append(5) # O(1)
>>> sorted_list.append(3) # O(1)
>>> sorted_list.append(1) # O(1)
>>> sorted_list.append(2) # O(1)
>>> sorted_list.sort()
# O(n * log(n)) = O(4 * log(4)) = O(8)
>>> sorted_list
[1, 2, 3, 5]
Using bisect:
>>> sorted_list = []
>>> bisect.insert(sorted_list, 5) # O(n) = O(1)
>>> bisect.insert(sorted_list, 3) # O(n) = O(2)
>>> bisect.insert(sorted_list, 1) # O(n) = O(3)
>>> bisect.insert(sorted_list, 2) # O(n) = O(4)
>>> sorted_list
[1, 2, 3, 5]
```
