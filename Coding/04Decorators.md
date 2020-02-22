# Why functools.wraps is important
Whenever you are writing a decorator, always be sure to add functools.wraps to wrap the inner function. Without wrapping it, you will lose all properties from the original function.
```python
>>> def eggs(function):
...     @functools.wraps(function)
...     def _eggs(*args, **kwargs):
...         return function(*args, **kwargs)
...     return _eggs
```

# Decorators with (optional) arguments
With arguments, you need three layers, but without arguments, you need only two layers.
```python
>>> def add(extra_n=1):
...'Add extra_n to the input of the decorated function'
...
...     # The inner function, notice that this is the actual
...     # decorator
...     def _add(function):
...     # The actual function that will be called
...         @functools.wraps(function)
...         def __add(n):
...             return function(n + extra_n)
...
...         return __add
...
...     return _add

# Creating decorators using classes
The only notable difference between functions and classes is that functools.wraps is now replaced with functools.update_wrapper in the __init__ method.
```python
>>> class Debug(object):
...
...def __init__(self, function):
... self.function = function
... # functools.wraps for classes
... functools.update_wrapper(self, function)
...
...def __call__(self, *args, **kwargs):
... output = self.function(*args, **kwargs)
... print('%s(%r, %r): %r' % (
...self.function.__name__, args, kwargs, output))
...return output

>>> @Debug
... def spam(eggs):
...     return 'spam' * (eggs % 5)
```
# Decorating class functions
It is similar to regular functions, different is that it required first argument 'self' - the class instance.
```python
>>> def plus_one(function):
...     @functools.wraps(function)
...     def _plus_one(self, n):
...         return function(self, n + 1)
...     return _plus_one
>>> class Spam(object):
...     @plus_one
...     def get_eggs(self, n=2):
...         return n * 'eggs'
```

## classmethod and staticmethod
The classmethod passes a class object (cls) instead of a class instance ( self ), and staticmethod skips both the class and the instance entirely.

## property
```python
>>> class Spam(object):
...
...def get_eggs(self):
...     print('getting eggs')
...     return self._eggs
...
...def set_eggs(self, eggs):
...     print('setting eggs to %s' % eggs)
...     self._eggs = eggs
...
...def delete_eggs(self):
...     print('deleting eggs')
...     del self._eggs
...
...eggs = property(get_eggs, set_eggs, delete_eggs)
...
... @property
... def spam(self):
...     print('getting spam')
...     return self._spam
...
... @spam.setter
... def spam(self, spam):
...     print('setting spam to %s' % spam)
...     self._spam = spam
...
... @spam.deleter
... def spam(self):
...     print('deleting spam')
...     del self._spam
```
# Decorating classes
## Singletons - classes with a single instance
Singletons are classes that always allow only a single instance to exist.
```python
>>> def singleton(cls):
...     instances = dict()
...     @functools.wraps(cls)
...     def _singleton(*args, **kwargs):
...         if cls not in instances:
...             instances[cls] = cls(*args, **kwargs)
            return instances[cls]
        return _singleton
>>> @singleton
... class Spam(object):
...     def __init__(self): 
            print('Executing init')
>>> a = Spam()
Executing init
>>> b = Spam()
>>> a is b
True
>>> a.x = 123
>>> b.x
123
```

## Total ordering – sortable classes the easy way
When use sorted function to the sorted function, often—by implementing the __gt__ , __ge__ , __lt__ , __le__ , and __eq__ functions. The total_ordering class decorator can implement all required sort functions based on a class that possesses an __eq__
function and **one** of the comparison functions ( __lt__ , __le__ , __gt__ , or __ge__ ). It can shorten function definitions.
```python
>>> class Spam(Value):
...     def __gt__(self, other):
...         return self.value > other.value
...
...     def __ge__(self, other):
...         return self.value >= other.value
...
...     def __lt__(self, other):
...         return self.value < other.value
...
...     def __le__(self, other):
...         return self.value <= other.value
...
...     def __eq__(self, other):
...         return self.value == other.value

>>> @functools.total_ordering
... class Egg(Value):
...     def __lt__(self, other):
...         return self.value < other.value
...
...     def __eq__(self, other):
...         return self.value == other.value
```

# Useful decorators
## Single dispatch - polymorphism in Python
Polymorphism means different functions being called depending on the argument types. 
```python
>>> @functools.singledispatch
... def printer(value):
...     print('other: %r' % value)
>>> @printer.register(str)
... def str_printer(value):
...     print(value)
>>> @printer.register(int)
... def int_printer(value):
...     printer('int: %d' % value)
>>> @printer.register(dict)
... def dict_printer(value):
...     printer('dict:')
...     for k, v in sorted(value.items()):
...         printer('key: %r, value: %r' % (k, v))
>>> printer('spam')
spam
>>> printer([1, 2, 3])
other: [1, 2, 3]
>>> printer(123)
int: 123
>>> printer({'a': 1, 'b': 2})
dict:
key: 'a', value: 1
key: 'b', value: 2
```
To check the registered types, you can access the registry, which is a dictionary,
through write_as_json.registry :

    >>> write_as_json.registry.keys()
    dict_keys([<class 'bytes'>, <class 'object'>, <class 'str'>])

## Contextmanager, with statements made easy
The standard method of creating a context manager is by creating a class that implements the __enter__ and __exit__ methods. With contextlib, we can have it simpler and shorter:
```python
>>> @contextlib.contextmanager
... def open_context_manager(filename, mode='r'):
...     fh = open(filename, mode)
...     yield fh
...     fh.close()

>>> with open_context_manager('test.txt', 'w') as fh:
...     print('Our test is complete!', file=fh)
```

It can also used as a ContextDecorator.
```python
>>> @contextlib.contextmanager
... def debug(name):
...     print('Debugging %r:' % name)
...     yield
...     print('End of debugging %r' % name)
>>> @debug('spam')
... def spam():
...     print('This is the inside of our spam function')
>>> spam()
Debugging 'spam':
This is the inside of our spam function
End of debugging 'spam'
```