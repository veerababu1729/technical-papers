# Python OOP Cheat Sheet

---

## Why classes?

Real applications are written with classes and objects, and coding rounds use them too.

- **Class** =  A blueprint or template that defines the structure (attributes and methods) for creating objects.
- **Object** = A concrete instance of a class that holds actual data and occupies memory.

One `Car` class, many car objects:

```
            Car (class)
        /       |        \
   red Ford   blue Toyota   green Golf     <- objects
```

Same blueprint, different values in each.

---

## Your first class

```python
class Student:
    def __init__(self, name, age, grade):    # constructor
        self.name = name
        self.age = age
        self.grade = grade

    def display_info(self):                  # method
        print(f"Name: {self.name}, Age: {self.age}, Grade: {self.grade}")


# creating objects
student1 = Student("Alice", 15, "10th")      # calls __init__
student2 = Student("Bob", 14, "9th")

# calling methods
student1.display_info()    # Name: Alice, Age: 15, Grade: 10th
student2.display_info()    # Name: Bob, Age: 14, Grade: 9th
```

Syntax:
```python
class ClassName:
    def __init__(self, params):
        self.variable = params

    def method_name(self):
        # statements

obj = ClassName(values)
obj.method_name()
```

### The parts

**`class`** starts the blueprint. Class names usually start with a capital letter.

**`__init__`** is the constructor. It runs automatically the moment you create an object, and sets up the object's starting values. Two underscores on each side. `_init_` with one underscore won't work.

**`self`** means "this particular object". Inside the class, `self.name` is the name of the object you're working on. Python passes `self` automatically, so you never type it when calling:
```python
student1.display_info()
# is the same as
Student.display_info(student1)
```

**Attributes** are the variables stored on an object (`self.name`, `self.age`).

**Methods** are functions inside a class. The first parameter is always `self`.

---

## Class without a constructor

Not every class needs `__init__`. If the class only does work (like a calculator) and doesn't store data, skip it.

```python
class MathOperations:
    def add(self, num1, num2):
        return num1 + num2

    def multiply(self, num1, num2):
        return num1 * num2


ob1 = MathOperations()

sum_result = ob1.add(10, 5)
product_result = ob1.multiply(10, 5)

print("Sum:", sum_result)          # Sum: 15
print("Product:", product_result)  # Product: 50
```

### The difference between the two examples

| | Student | MathOperations |
|---|---------|----------------|
| Constructor | yes, stores name, age, grade | no |
| Data kept in object | yes | no |
| Method inputs | uses stored data via `self` | gets data as arguments |

Use a constructor when the object should remember something. Use plain methods when each call brings its own input.

---

## Changing attributes

```python
s = Student("Alice", 15, "9th")
print(s.grade)       # 9th
s.grade = "10th"
print(s.grade)       # 10th
```

---

## Default values (extra)

```python
class BankAccount:
    def __init__(self, balance=0):       # balance is 0 unless you pass one
        self.balance = balance

    def deposit(self, amount):
        self.balance += amount
        print("Deposited:", amount, "| Balance:", self.balance)

    def withdraw(self, amount):
        if amount > self.balance:
            print("Not enough balance")
        else:
            self.balance -= amount
            print("Withdrawn:", amount, "| Balance:", self.balance)


acc = BankAccount()
acc.deposit(500)      # Deposited: 500 | Balance: 500
acc.withdraw(200)     # Withdrawn: 200 | Balance: 300
```

---

## Return vs print inside methods (extra)

```python
class Car:
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model

    def show(self):
        return f"Car Details: {self.brand} {self.model}"


c = Car("Toyota", "Corolla")
print(c.show())    # Car Details: Toyota Corolla
```

Returning gives you a value you can store and reuse. Printing only shows it.

---

## Instance attributes vs class attributes (extra)

```python
class Dog:
    species = "Mammal"            # class attribute, shared by every Dog

    def __init__(self, name):
        self.name = name          # instance attribute, belongs to this dog


d1 = Dog("Max")
d2 = Dog("Bella")

print(d1.species, d1.name)    # Mammal Max
print(d2.species, d2.name)    # Mammal Bella
```

Change `Dog.species` and every dog sees the change. Change `d1.name` and only `d1` is affected.

---

## Making objects print nicely (extra)

```python
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def __str__(self):
        return f"{self.name}, {self.age}"


p = Person("Alice", 25)
print(p)    # Alice, 25
```

Without `__str__`, `print(p)` shows something like `<__main__.Person object at 0x...>`.

---

## Multiple objects (extra)

```python
class Book:
    def __init__(self, title, author):
        self.title = title
        self.author = author

books = [
    Book("The Hobbit", "J.R.R. Tolkien"),
    Book("The Alchemist", "Paulo Coelho"),
    Book("Dune", "Frank Herbert"),
]

for b in books:
    print(f"Title: {b.title}, Author: {b.author}")
```

---

## The four pillars of OOP (extra)

The book stops at classes and objects. These four ideas are what "OOP" means in interviews.

### 1. Encapsulation:  
Encapsulation is the practice of binding the data (variables) and the code (methods) together into a single unit (a class), while restricting direct access to some of the object's components.  

Keep data and the methods that work on it together, and protect the data from careless changes.

Python uses naming conventions:
- `name` is public
- `_name` means "internal, please don't touch"
- `__name` is name-mangled, so it's harder to reach from outside

```python
class Account:
    def __init__(self, balance):
        self.__balance = balance          # private

    def get_balance(self):
        return self.__balance

    def deposit(self, amount):
        if amount > 0:
            self.__balance += amount


a = Account(100)
a.deposit(50)
print(a.get_balance())    # 150
print(a.__balance)        # AttributeError
```

Python doesn't truly lock anything. `a._Account__balance` still works. It's a convention, not a wall.

### 2. Inheritance  
Inheritance allows a new class (child/subclass) to adopt/resuse the attributes and methods of an existing class (parent/superclass).  
A child class reuses what a parent class already has.

```python
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"{self.name} is eating")


class Dog(Animal):                  # Dog inherits from Animal
    def bark(self):
        print(f"{self.name} says woof")


d = Dog("Max")
d.eat()     # Max is eating   (from Animal)
d.bark()    # Max says woof   (from Dog)
```

If the child has its own `__init__`, call the parent's with `super()`:
```python
class Cat(Animal):
    def __init__(self, name, indoor):
        super().__init__(name)      # sets self.name
        self.indoor = indoor
```

### 3. Polymorphism  
Polymorphism means "many forms." It allows different classes to be treated as if they are the same type, letting the same method execution behave differently depending on the object calling it.  
1. compile time: Method overloading.  (same methods with different arguemnts presented within a class)  
2. run time: Method overriding.   (child class overrides methods of parent class)  
Same method name, different behaviour depending on the object. A child overrides the parent's method:

```python
class Animal:
    def speak(self):
        print("Some sound")

class Dog(Animal):
    def speak(self):
        print("Woof")

class Cat(Animal):
    def speak(self):
        print("Meow")


for pet in [Dog(), Cat(), Animal()]:
    pet.speak()
# Woof
# Meow
# Some sound
```

The loop doesn't care which type each pet is. It just calls `speak()`.

### 4. Abstraction  
Abstraction is the practice of hiding the complex internal implementation details and showing only the essential features to the user.  
Show what something does, hide how it does it. In Python you build a blueprint that other classes must complete.

```python
from abc import ABC, abstractmethod

class Shape(ABC):
    @abstractmethod
    def area(self):
        pass


class Rectangle(Shape):
    def __init__(self, l, b):
        self.l = l
        self.b = b

    def area(self):
        return self.l * self.b


# Shape()                      # TypeError, can't create an abstract class
print(Rectangle(5, 10).area())  # 50
```

If a child skips an abstract method, Python won't let you create it.

---

## Quick glossary

| Term | Meaning |
|------|---------|
| Class | blueprint |
| Object / instance | one thing built from the class |
| Attribute | variable stored on an object or class |
| Method | function inside a class |
| Constructor (`__init__`) | runs when the object is created |
| `self` | the object currently being used |
| Encapsulation | bundle data with methods, protect it |
| Inheritance | child class reuses a parent class |
| Polymorphism | same method name, different behaviour |
| Abstraction | hide the details, expose the essentials |

---

## Common mistakes

- Forgetting `self` as the first parameter: `TypeError: takes 1 positional argument but 2 were given`
- Writing `_init_` instead of `__init__`: the constructor never runs
- Forgetting `self.` inside the class: `name = name` does nothing useful. It should be `self.name = name`.
- Calling a method on the class, not the object: use `obj.method()`, not `Class.method()`
- Expecting changes to one object to show up in another. Each object has its own attributes. Only class attributes are shared.
