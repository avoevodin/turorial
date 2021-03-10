# Tutorial for Object-Oriented Programming

## Classes and Objects:

### Classes
> A description of attributes and behavior for the objects.
* Example:
```python
class SomeClass(object):
    attr1 = 24

    def method1(self, x):
        return 2*x
```
* Example of dynamic change:
```python
class SomeClass(object):
    pass

def squareMethod(self, x):
    return x*x

SomeClass.square = squareMethod
obj = SomeClass()
obj.square(5) # 25
```

### Objects
> An instance with its own state of class properties.
* Example:
```python
obj = SomeClass()
obj.method1(6) # 12
obj.attr1 # 24
obj.attr1 = 48 #48
```

### Basic concepts
* self - special variable that allows object to manipulate with its own 
  attributes inside its own methods.
  
* \_\_init\_\_ - this method automatically is called when the object is creating.
```python
class Point(object):
    def __init__(self, x, y, z):
        self.coord = (x, y, z)

p = Point(13, 14, 15)
p.coord # (13, 14, 15)
```
* \_\_new\_\_ - is the first step of instance creation. It's called first, 
  and is responsible for returning a new instance of your class.
  In contrast, \_\_init\_\_ doesn't return anything; it's only responsible 
  for initializing the instance after it's been created.
```python
class SomeClass(object):
    def __new__(cls):
        print("new")
        return super(SomeClass, cls).__new__(cls)

    def __init__(self):
        print("init")

obj = SomeClass();
# new
# init
```
* Static method - method that doesn't operate with internal state of object
  and doesn't require creating of object.
```python
class SomeClass(object):
    @staticmethod
    def hello():
        print("Hello, world")

SomeClass.hello() # Hello, world
obj = SomeClass()
obj.hello() # Hello, world
```
* Class method - method that's executed not in the context
  of the object, but in the context of the class itself.
```python
class SomeClass(object):
    @classmethod
    def hello(cls):
        print('Hello, class {}'.format(cls.__name__))

SomeClass.hello() # Hello, class SomeClass
```

## Association
> When one object is a part of second object, it's called an association.
* Composition - is an association when a child object lifecycle is similar
with a parent object lifecycle.
```python
class Salary:
    def __init__(self, pay):
        self.pay = pay

    def getTotal(self):
        return (self.pay * 12)

class Employee:
    def __init__(self, pay, bonus):
        self.pay = pay
        self.bonus = bonus
        self.salary = Salary(self.pay)

    def annualSalary(self):
        return "Total: " + str(self.salary.getTotal() + self.bonus)

employee = Employee(100,10)
print(employee.annualSalary()) # Total: 1210
```  
* Aggregation - is an association when a child object lifcycle doesn't depend
  on a parent object lifecycle.
```python
class Salary(object):
    def __init__(self, pay):
        self.pay = pay

    def getTotal(self):
        return (self.pay * 12)

class Employee(object):
    def __init__(self, pay, bonus):
        self.pay = pay
        self.bonus = bonus

    def annualSalary(self):
        return "Total: " + str(self.pay.getTotal() + self.bonus)

salary = Salary(100)
employee = Employee(salary, 10)
print(employee.annualSalary()) # Total: 1210
```
## Inheritance
> Is a mechanism that provide classes to inherit properties 
> and behavior of another classes.
> </br>Inheritance is static.
* Example:
```python
class Mammal():
    className = 'Mammal'

class Dog(Mammal):
    species = 'Canis lupus'

dog = Dog()
dog.className # Mammal
```
* Example of overload:
> Methods is called from the end of hierarchy.
```python
class Mammal:
    className = "Mammal"

    def drink_milk(self, litres):
        return "Mammals drink milk about " + litres + " litres \
            per hour."


class Dog(Mammal):
    species = "Canis lupus"

    def drink_milk(self, litres):
        return "Dogs drink milk about " + litres + " litres \
            per hour."


dog = Dog()
dog.drink_milk(5)  # Dogs drink milk about 5 litres per hour.
```
* Multiple Inheritance:
> Is an inheritance, when the child has more than one parent.
> Multiple Inheritance isn't recommended to use, but may
> be used for "mix in" mechanism.
```python
class Horse():
    isHorse = True

class Donkey():
    isDonkey = True

class Mule(Horse, Donkey):
    mule = Mule()
    mule.isHorse # True
    mule.isDonkey # True
```
* Life-hack for making a choice between
  composition and Inheritance:
  > If the first object is a kind of second, there is an inheritance.
  > </br> If the first object is a part of second, there is a composition.
* Abstract classes:
> Classes that don't provide creating of objects of these classes.
> Abstract classes may content signatures of methods without implementations.

## Polymorphism
> All methods in Python basically are virtual. Child classes may redefine
> methods of parents classes.
* Example:
```python
class Mammal:
    def move(self):
        print('Moves')

class Hare(Mammal):
    def move(self):
        print('Jumps')

animal = Mammal()
animal.move() # Moves
hare = Hare()
hare.move() # Jumps
```

* Calling methods of parent class 
  directly and with the super method:
```python
class Parent():
    def __init__(self):
        print('Parent init')

    def method(self):
        print('Parent method')

class Child(Parent):
    def __init__(self):
        Parent.__init__(self)

    def method(self):
        super(Child, self).method()

child = Child() # Parent init
child.method() # Parent method
```

## Encapsulation
> Access control for fields and methods of an object
* Example without hiding:
```python
class SomeClass:
    def _private(self):
        print("This is the internal method of the object")

obj = SomeClass()
obj._private() # This is the internal method of the object
```
* Example with hiding:
```python
class SomeClass():
    def __init__(self):
        self.__param = 42 # protected attribute

obj = SomeClass()
obj.__param # AttributeError: 'SomeClass' object has no attribute '__param'
obj._SomeClass__param # 42
```
* Getters, Setters and Destructors
```python
class SomeClass():
    def __init__(self, value):
        self._value = value

    def getvalue(self): # получение значения атрибута
        return self._value

    def setvalue(self, value): # установка значения атрибута
        self._value = value

    def delvalue(self): # удаление атрибута
        del self._value

    value = property(getvalue, setvalue, delvalue, "Property value")

obj = SomeClass(42)
print(obj.value)
obj.value = 43
```

## Metaclasses
> Instance of a metaclass is a class.
```python
class MetaClass(type):
    # allocating memory for a class
    def __new__(cls, name, bases, dict):
        print("Creating new class {}".format(name))
        return type.__new__(cls, name, bases, dict)

    # инициализация класса
    def __init__(cls, name, bases, dict):
        print("Initializing of new class {}".format(name))
        return super(MetaClass, cls).__init__(name, bases, dict)

# spawning a class based on a metaclass
SomeClass = MetaClass("SomeClass", (), {})

# usual inheritance
class Child(SomeClass):
    def __init__(self, param):
        print(param)

# making an instance of new class
obj = Child("Hello")
```

## List of sources used
> https://habr.com/ru/post/463125/
> <br/>https://proglib.io/p/python-oop/