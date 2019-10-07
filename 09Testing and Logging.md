# pytest

1. The way to assert is different: In pytest, only assert ... == ...
```py
class TestPyCube(object):
def test_0(self):
    assert cube.cube(0) == 0  # In unittest, it should be self.assert cube.cube(0) == 0
def test_no_arguments(self):
    with pytest.raises(TypeError):
        cube.cube()
```
2. Result of pytest will be more detailed. It also show the input parameters the function received.

3. Test should not need to encapsulate in a class with pytest.

4. parameterize tests: one kind similar tests driven by parameters
```py
import pytest
import cube
cubes = (
    (0, 0),
    (1, 1),
    (2, 8),
    (3, 27),
)

# four test simplified with parametrize
@pytest.mark.parametrize('n,expected', cubes)
def test_cube(n, expected):
    assert cube.cube(n) == expected
```

5. pytest use plugins to expand its function, plugins' configuration is maintained with pytest.ini file.
```ini
[pytest]
python_files =
    your_project_source/*.py
    tests/*.py
addopts =
    --doctest-modules
    --cov your_project_source
    --cov-report term-missing
    --cov-report html
    --pep8
    --flakes
# W391 is the error about blank lines at the end of a file
pep8ignore =
    *.py W391
# Ignore unused imports
flakes-ignore =
    *.py UnusedImport
```

# Mock in unittest
With Mock class, you can make assertions about which methods / attributes were used and arguments they were called with. You can also specify return values and set needed attributes in the normal way. Additionally, mock provides a patch() decorator that handles patching module and class level attributes within the scope of a test, along with sentinel for creating unique objects. [Details](https://docs.python.org/3/library/unittest.mock.html#patch-dict)

1. return_value used to specify return value of function
```py
>>> from unittest.mock import MagicMock
>>> thing = ProductionClass()
>>> thing.method = MagicMock(return_value=3)
>>> thing.method(3, 4, 5, key='value')
3
```
2. side_effect can define a return value list, raise an exception when a mock is called.
```py
>>> mock = Mock(side_effect=KeyError('foo'))
>>> mock()
Traceback (most recent call last):
 ...
KeyError: 'foo'

>>> values = {'a': 1, 'b': 2, 'c': 3}
>>> def side_effect(arg):
...     return values[arg]
...
>>> mock.side_effect = side_effect
>>> mock('a'), mock('b'), mock('c')
(1, 2, 3)
>>> mock.side_effect = [5, 4, 3, 2, 1]
>>> mock(), mock(), mock()
(5, 4, 3)
```

3. The patch() decorator / context manager makes it easy to mock classes or objects in a module under test. 
```py
>>> from unittest.mock import patch
>>> @patch('module.ClassName2')
... @patch('module.ClassName1')
... def test(MockClass1, MockClass2):
...     module.ClassName1()
...     module.ClassName2()
...     assert MockClass1 is module.ClassName1
...     assert MockClass2 is module.ClassName2
...     assert MockClass1.called
...     assert MockClass2.called
...
>>> test()

>>> with patch.object(ProductionClass, 'method', return_value=None) as mock_method:
...     thing = ProductionClass()
...     thing.method(1, 2, 3)
...
>>> mock_method.assert_called_once_with(1, 2, 3)
```
4. There is also patch.dict() for setting values in a dictionary just during a scope and restoring the dictionary to its original state when the test ends:
```py
>>> foo = {'key': 'value'}
>>> original = foo.copy()
>>> with patch.dict(foo, {'newkey': 'newvalue'}, clear=True):
...     assert foo == {'newkey': 'newvalue'}
...
>>> assert foo == original
```
5. Mock supports the mocking of Python magic methods. 
```py
>>> mock = MagicMock()
>>> mock.__str__.return_value = 'foobarbaz'
>>> str(mock)
'foobarbaz'
>>> mock.__str__.assert_called_with()

>>> mock = Mock()
>>> mock.__str__ = Mock(return_value='wheeeeee')
>>> str(mock)
'wheeeeee'
```
6. Auto-speccing creates mock objects that have the same attributes and methods as the objects they are replacing, and any functions and methods (including constructors) have the same call signature as the real object.
```py
>>> from unittest.mock import create_autospec
>>> def function(a, b, c):
...     pass
...
>>> mock_function = create_autospec(function, return_value='fishy')
>>> mock_function(1, 2, 3)
'fishy'
>>> mock_function.assert_called_once_with(1, 2, 3)
>>> mock_function('wrong arguments')
Traceback (most recent call last):
 ...
TypeError: <lambda>() takes exactly 3 arguments (1 given)
```
create_autospec() can also be used on classes, where it copies the signature of the __init__ method, and on callable objects where it copies the signature of the __call__ method.
