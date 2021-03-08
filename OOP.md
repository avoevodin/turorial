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

### Objects
> An instance with its own state of class' properties.
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
  
* __init__ - this method automatically is called when the object is creating.
```python
class Point(object):
    def __init__(self, x, y, z):
        self.coord = (x, y, z)

p = Point(13, 14, 15)
p.coord # (13, 14, 15)
```
* __new__ - is the first step of instance creation. It's called first, 
  and is responsible for returning a new instance of your class.
  In contrast, __init__ doesn't return anything; it's only responsible 
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
* Dynamic method - method that operates with internal state of object.
  
* Static method - method that doesn't operate with internal state of object
  and doesn't require creating of object.

## Association
> When one object is a part of second object, it's called the association.
* Composition - is an association when a child object lifecycle is similar
with a parent object lifecycle.
  
* Aggregation - is an association when a child object lifcycle doesn't depend
  on a parent object lifecycle.

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
* Example of reload:
> Methods is called from the end of hierarchy.
```python
class Mammal():
    className = 'Mammal'
    
    def drink_milk(self, litres):
        return "Mammals drink milk." 
        
class Dog(Mammal):
    species = 'Canis lupus'
    
    def drink_milk(self, litres):
        return "Dogs drink milk."

dog = Dog()
dog.drink_milk() # Dogs drink milk.
```
* Life-hack for making a choice between
  composition and Inheritance:
  > If the first object is a kind of second, there is an inheritance.
  > </br> If the first object is a part of second, there is a composition.
* Multiple Inheritance:
> Is an inheritance, when the child has more than one parent.
> Multiple Inheritance isn't recommended to use, but may
> be used for "mix in" mechanism.
* Abstract classes:
> Classes that don't provide creating of objects of these classes.
> Abstract classes may content signatures of methods without implementations.

## Polymorphism
> ALl methods in Python basically are virtual. Child classes may redefine
> methods of parents classes.
```python
class Mammal:
    def move(self):
        print('Двигается')

class Hare(Mammal):
    def move(self):
        print('Прыгает')

animal = Mammal()
animal.move() # Двигается
hare = Hare()
hare.move() # Прыгает
```

## List of sources used
> https://habr.com/ru/post/463125/
> <br/>https://proglib.io/p/python-oop/