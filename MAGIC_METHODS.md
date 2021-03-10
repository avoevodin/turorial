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