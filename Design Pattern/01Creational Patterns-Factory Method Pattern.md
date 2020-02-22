**The “Factory Method” pattern is a poor fit for Python. It was designed for underpowered programming languages where classes and functions can’t be passed as parameters or stored as attributes. In those languages, the Factory Method serves as an awkward but necessary escape route. But it’s not a good design for Python applications.**

**You should wait and consider providing the class through Dependency Injection instead of using 'Factory Method' pattern.**

# Useage
Use a factory class to create new different classes.

# Way to realize

1. use a Class Attribute Factory


We can change the HTTPConnection's attribute `response_class` to make a new class which behavior is different.
```python
class HTTPConnection:
    ...
    response_class = HTTPResponse
    ...
    def getresponse(self):
        ...
        response = self.response_class(self.sock, method=self._method)
        ...

class SpecialHTTPConnection(HTTPConnection):
    response_class = SpecialHTTPResponse
```     

2. use an Instance Attribute Factory

By offering a different parameter during instatiatation, new type class is created.
```python
class JSONDecoder(object):
    ...
    def __init__(self, ... parse_float=None, ...):
        ...
        self.parse_float = parse_float or float
        ...

from decimal import Decimal

my_decoder = JSONDecoder(parse_float=Decimal)
```

3. Instance attributes override class attributes

Override the class attributes can create a new class.
```
conn = HTTPConnection()
conn.response_class = SpecialHTTPResponse
```