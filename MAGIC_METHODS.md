# Tutorial for Magic Methods in Python

## Constructing and initializing
* ```__new__(cls, [...)``` - the first method of object initializing.
* ```__init__(self, [...)``` - class initializer.
* ```__del__(self, [...)``` - object destructor. It may define a behavior
    of an object while object is in a garbage collector.

```python
from os.path import join


class FileObject:
    """Wrapper for a file-object to be sure the file
        will be closed after deleting"""

    def __init__(self, filepath='~', filename='sample.txt'):
        # open filename in filepath in read and write mode
        self.file = open(join(filepath, filename), 'r+')

    def __del__(self):
        self.file.close()
        del self.file
```

## Comparison operators
> Methods for operators of comparison.

* ```__cmp__``` - returns 0 if self == other, -1 if self < other and 1 if 
  self > other.
* ```__eq__ and __ne__``` - define '==' and '!=' operators.
* Separate methods for comparison operators:
```python
class Word(str):
    """Class for words, that defines comparison by the length."""
    def __new__(cls, word):
        # We should use __new__, because the type str is immutable.
        # We should initialize it earlier, while it's creating.
        if ' ' in word:
            print("Value contains spaces. Truncating to first space.")
            word = word[:word.index(' ')]  # Now Word is all symbols 
                                           # before the first space
        return str.__new__(cls, word)

    def __gt__(self, other):
        return len(self) > len(other)

    def __lt__(self, other):
        return len(self) < len(other)

    def __ge__(self, other):
        return len(self) >= len(other)

    def __le__(self, other):
        return len(self) <= len(other)
```

## Numeric Magic Methods

### Unary operators and functions.
> These methods have only one operand.
> </br>```__pos__(self)```, ```__neg__(self)```, ```__abs__(self)``` etc.

### Usual arithmetic operators.
> ```__add__(self, other)```, ```__sub__(self, other)```, 
> ```__mul__(self, other)``` etc.

### Reflected arithmetic operators.
> Order of operands is reversed in comparison with usual operators.
> ```__radd__(self, other)```, ```__rsub__(self, other)```, 
> ```__rmul__(self, other)``` etc.

* NOTICE:
> For example ```__radd__``` will be called if only there isn't 
> the operator ```__add__``` in the 'other' operand:
> <br/>other + self

### Composite assignment
> Ex x += 1
> <br/>```__iadd__(self, other)```, ```__isub__(self, other)```, 
  ```__imul__(self, other)``` etc.
  
### Type conversion
> <br/>```__int__(self)```, ```__long__(self)```, ```__float___(self)``` etc.

## Classes representation
* ```__str__(self)``` - behavior for str() function. It's usually used by users.
* ```__repr__(self)``` - behaviour for repr() function. It's usually used by
  machine-oriented print out.
* ```__unicode__(self)``` - behavior for unicode() function. Returns str 
  in Unicode.
> etc.

## Attributes access control
* ```__getattr__(self, name)``` - getter for not existed attributes.
* ```__setattr__(self, name)``` - setter for all attributes.
* ```__delattr__(self, name)``` - setter for deleting for all attributes.
* ```__getattribute__(self, name)``` - getter for all attributes.
-------------
1. Example of use and misuse in redefining of access control methods:
```python
def __setattr__(self, name, value):
    self.name = value
    # There will be an infinite recursion.

def __setattr__(self, name, value):
    self.__dict__[name] = value # assingment to dict of class variables
    # next define any behavior 
```
2. Example of access control:
> super method is used, because the attribute __dict__ isn't
> enclosed in every class.
```python
class AccessCounter(object):
    """Class with attribute value. Counter increases with
    every change of value.
    
    """
    def __init__(self, val):
        super(AccessCounter, self).__setattr__('counter', 0)
        super(AccessCounter, self).__setattr__('value', val)

    def __setattr__(self, name, value):
        if name == 'value':
            super(AccessCounter, self).__setattr__('counter', self.counter + 1)
        # There may be for example AttributeError(name)
        super(AccessCounter, self).__setattr__(name, value)

    def __delattr__(self, name):
        if name == 'value':
            super(AccessCounter, self).__setattr__('counter', self.counter + 1)
        super(AccessCounter, self).__delattr__(name)
```

##  Creating of Arbitrary sequences 

### Protocols

* Not changeable container should enclose:
> ```__len__``` and ```__getitem__```
* Changeable container:
> The same with not changeable and ```__setitem__``` and ```__delitem__```.
* Iterable container:
> The same with changeable container and ```__iter__```

### Example of Functional List:
```python
class FunctionalList:
    """Class-wrapper above the list with some add functional: head, tail,
        init, last, drop, take.
    
    """
    def __init__(self, values=None):
        if values is None:
            self.values = []
        else:
            self.values = values

    def __len__(self):
        return len(self.values)

    def __getitem__(self, key):
        # if type or value of the key are Uncorrected,
        # list will raise an error
        return self.values[key]

    def __setitem__(self, key, value):
        self.values[key] = value

    def __delitem__(self, key):
        del self.values[key]

    def __iter__(self):
        return iter(self.values)

    def __reversed__(self):
        return FunctionalList(reversed(self.values))

    def append(self, value):
        self.values.append(value)
    def head(self):
        # get the first element
        return self.values[0]
    def tail(self):
        # get all elements after the first
        return self.values[1:]
    def init(self):
        # get all elements except of last
        return self.values[:-1]
    def last(self):
        # get the last element
        return self.values[-1]
    def drop(self, n):
        # all elements except of first n
        return self.values[n:]
    def take(self, n):
        # first n elements
        return self.values[:n]
```

## Reflection
* __instancecheck__(self, instance) - function isinstance() checks if the
    current instance is an instance of your class.
* __subclasscheck__(self, subclass) - function issubclass() checks if the
    current class is your class child.
  
## Called objects
* __call__(self, [args...]) - let every instance of the class 
  be called like a function.
```python
class Entity:
    """Class describes object on the plane. Object can be called
        to update its position.
        
    """
    def __init__(self, size, x, y):
        self.x, self.y = x, y
        self.size = size

    def __call__(self, x, y):
        """Move object to selected coords."""
        self.x, self.y = x, y

test_obj = Entity(0, 0)
test_obj(100, 100) # move object to (100, 100)
```

## Context manager
* Example of context manager:
```python
with open('foo.txt') as bar:
    # some actions with bar
```

* Magic methods for context manager:
    1. __enter__(self) - defines actions for the beginning of the CM-block.
        Method returns the main value for operating into the CM-block.
    2. __exit__(self, exception_type, exception_value, traceback) - defines
        actions after completing CM-block or after it getting interrupted.
       
```python
class Closer:
    """Context manager for auto close object with close-method in
        with-sentence.
    
    """
    def __init__(self, obj):
        self.obj = obj

    def __enter__(self):
        return self.obj # assign to an active object of with-block

    def __exit__(self, exception_type, exception_val, trace):
        try:
           self.obj.close()
        except AttributeError: # object hasn't close-method
           print("Not closable.")
           return True # except is caught
```
```shell
>>> from magicmethods import Closer
>>> from ftplib import FTP
>>> with Closer(FTP('ftp.somesite.com')) as conn:
...     conn.dir()
...
# output omitted for brevity
>>> conn.dir()
# long AttributeError message, can't use a connection that's closed
>>> with Closer(int(5)) as i:
...     i += 1
...
Not closable.
>>> i
6
```
