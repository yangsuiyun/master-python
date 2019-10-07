# Introduce to metaclass
When creating classes in a procedural way, the *type* metaclass is used as a function. The *name* will become the __name__ attribute, the *bases* is the list of inherited base classes and will be stored in __bases__ and *dict* is the namespace dictionary that contains all variables and will be stored in __dict__ . Following is different ways to create a same class. When creating a class, Python silently adds the *type* metaclass, just like the third Spam definition.
```python
>>> class Spam(object):
>>>     eggs = 'my eggs'
>>> Spam = type('Spam', (object,), dict(eggs='my eggs'))
>>> class Spam(object, metaclass=type):
>>>     eggs = 'my eggs'
```
# use metaclass change behavior of class creating
```py
# The metaclass definition, note the inheritance of type instead
# of object
>>> class MetaSpam(type):
...
... # Notice how the __new__ method has the same arguments
... # as the type function we used earlier?
... def __new__(metaclass, name, bases, namespace):
...     name = 'SpamCreatedByMeta'
...     bases = (int,) + bases
...     namespace['eggs'] = 1
...     return type.__new__(metaclass, name, bases, namespace)
# First, the regular Spam:
>>> class Spam(object):
...     pass
>>> Spam.__name__
'Spam'
>>> issubclass(Spam, int)
False
>>> Spam.eggs
Traceback (most recent call last):
... AttributeError: type object 'Spam' has no attribute 'eggs'
# Now the meta-Spam
>>> class Spam(object, metaclass=MetaSpam):
...     pass
>>> Spam.__name__
'SpamCreatedByMeta'
>>> issubclass(Spam, int)
True
>>> Spam.eggs
1
```