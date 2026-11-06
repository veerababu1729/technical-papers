# SOLID Principles Cheat Sheet

SOLID is five habits for writing classes that are easy to change without breaking things.

| Letter | Principle | One line |
|--------|-----------|----------|
| S | Single Responsibility | one class, one job |
| O | Open/Closed | add new behaviour without editing old code |
| L | Liskov Substitution | a child must work wherever its parent works |
| I | Interface Segregation | don't force classes to implement what they don't use |
| D | Dependency Inversion | depend on abstractions, pass dependencies in |

Prerequisite: classes, inheritance, and `ABC` from the OOP sheet.

---

## S: Single Responsibility

A class should have **one reason to change**.

**Bad.** One class does three jobs:
```python
class Order:
    def __init__(self, items):
        self.items = items

    def total(self):
        return sum(self.items)

    def save_to_db(self):
        print("Saving order to database...")

    def send_email(self):
        print("Sending confirmation email...")
```
Change the database and you edit `Order`. Change the email text and you edit `Order`. Everything is tangled together.

**Good.** One job each:
```python
class Order:
    def __init__(self, items):
        self.items = items

    def total(self):
        return sum(self.items)


class OrderRepository:
    def save(self, order):
        print("Saving order to database...")


class EmailService:
    def send_confirmation(self, order):
        print("Sending confirmation email...")
```

```python
order = Order([100, 250, 50])
OrderRepository().save(order)
EmailService().send_confirmation(order)
```

**Check yourself:** can you describe the class without using "and"? "It calculates the total **and** saves **and** emails" means split it.

---

## O: Open/Closed

Code should be **open for extension, closed for modification**. You add new features by adding new code, not by editing working code.

**Bad.** Every new customer type means editing this function:
```python
def get_discount(customer_type, price):
    if customer_type == "regular":
        return price * 0.05
    elif customer_type == "premium":
        return price * 0.10
    elif customer_type == "student":
        return price * 0.15
```
Every edit risks breaking the existing branches.

**Good.** Each discount is its own class:
```python
class Discount:
    def apply(self, price):
        raise NotImplementedError


class RegularDiscount(Discount):
    def apply(self, price):
        return price * 0.05


class PremiumDiscount(Discount):
    def apply(self, price):
        return price * 0.10


class StudentDiscount(Discount):
    def apply(self, price):
        return price * 0.15


def final_price(price, discount):
    return price - discount.apply(price)


print(final_price(1000, PremiumDiscount()))    # 900.0
```

A new `FestivalDiscount` class means a new class and nothing else. `final_price` never changes.

**Check yourself:** an `if/elif` chain that keeps growing as new types appear is the usual sign.

---

## L: Liskov Substitution

If code works with a parent class, it should work with **any child class** too, without surprises.

**Bad.** The child breaks the parent's promise:
```python
class Bird:
    def fly(self):
        print("Flying")


class Sparrow(Bird):
    pass


class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins can't fly")


def make_it_fly(bird):
    bird.fly()

make_it_fly(Sparrow())    # works
make_it_fly(Penguin())    # crashes
```
`Bird` says every bird flies. `Penguin` is a bird but breaks that promise.

**Good.** Don't promise what you can't keep:
```python
class Bird:
    def eat(self):
        print("Eating")


class FlyingBird(Bird):
    def fly(self):
        print("Flying")


class Sparrow(FlyingBird):
    pass


class Penguin(Bird):
    pass


def make_it_fly(bird: FlyingBird):
    bird.fly()

make_it_fly(Sparrow())    # works
# Penguin never gets here, because it isn't a FlyingBird
```

**Rules of thumb:**
- A child shouldn't raise errors the parent never raises.
- A child shouldn't do less than the parent promised.
- If you find yourself overriding a method just to make it fail or do nothing, the inheritance is wrong.

---

## I: Interface Segregation

Don't force a class to implement methods it doesn't use. Prefer several small interfaces over one big one.

**Bad.** One fat interface:
```python
from abc import ABC, abstractmethod

class Machine(ABC):
    @abstractmethod
    def print_doc(self): ...

    @abstractmethod
    def scan_doc(self): ...

    @abstractmethod
    def fax_doc(self): ...


class BasicPrinter(Machine):
    def print_doc(self):
        print("Printing")

    def scan_doc(self):
        raise NotImplementedError    # can't scan

    def fax_doc(self):
        raise NotImplementedError    # can't fax
```
`BasicPrinter` is stuck with two methods it can't do.

**Good.** Small interfaces, pick what you need:
```python
from abc import ABC, abstractmethod

class Printer(ABC):
    @abstractmethod
    def print_doc(self): ...


class Scanner(ABC):
    @abstractmethod
    def scan_doc(self): ...


class BasicPrinter(Printer):
    def print_doc(self):
        print("Printing")


class AllInOne(Printer, Scanner):
    def print_doc(self):
        print("Printing")

    def scan_doc(self):
        print("Scanning")
```

**Check yourself:** methods that just `raise NotImplementedError` or `pass` in a child mean the interface is too big.

In Python this is less strict than in Java or C#, because duck typing lets you skip formal interfaces. The idea still applies: keep your base classes small and focused.

---

## D: Dependency Inversion

High-level code shouldn't depend on low-level details. Both should depend on an abstraction. In practice: **don't create your dependencies inside a class, pass them in.**

**Bad.** The service builds its own email sender:
```python
class EmailSender:
    def send(self, message):
        print(f"Email: {message}")


class OrderService:
    def __init__(self):
        self.sender = EmailSender()       # locked to email

    def place_order(self):
        self.sender.send("Order placed")
```
Want SMS instead? You edit `OrderService`. Want to test it without sending real emails? You can't.

**Good.** Pass the sender in:
```python
class EmailSender:
    def send(self, message):
        print(f"Email: {message}")


class SMSSender:
    def send(self, message):
        print(f"SMS: {message}")


class OrderService:
    def __init__(self, sender):
        self.sender = sender              # anything with a send() method

    def place_order(self):
        self.sender.send("Order placed")


OrderService(EmailSender()).place_order()    # Email: Order placed
OrderService(SMSSender()).place_order()      # SMS: Order placed
```

Testing becomes easy with a fake:
```python
class FakeSender:
    def __init__(self):
        self.sent = []

    def send(self, message):
        self.sent.append(message)


fake = FakeSender()
OrderService(fake).place_order()
print(fake.sent)    # ['Order placed']
```

`OrderService` only cares that the sender has a `send()` method. It doesn't care which one.

---

## How they work together

One small example: order processing.

- **S**: `Order`, `OrderRepository` and `EmailService` each do one job.
- **O**: new discount types are new classes.
- **L**: every discount works wherever `final_price` expects a `Discount`.
- **I**: `Printer` and `Scanner` stay separate.
- **D**: `OrderService` receives its sender instead of making one.

---

## Don't overdo it

SOLID is a set of guidelines, not laws. Following it blindly gives you code with ten tiny classes for a ten-line script.

- Small script or one-off job: keep it simple.
- Don't add an abstraction before there's a second use for it. Wait until the code actually needs to change.
- Apply SOLID when a file is growing, bugs appear every time you change something, or tests are hard to write.

---

## Quick checklist

Ask yourself:

| Question | If "yes", think about |
|----------|-----------------------|
| Does this class do more than one job? | S: split it |
| Am I editing old code to add a new case? | O: add a class instead |
| Does a child raise errors or disable parent methods? | L: rework the inheritance |
| Are there methods a child can't implement? | I: split the interface |
| Does the class create its own tools inside? | D: pass them in |

---

## Common mistakes

- **Treating S as "one method per class".** It means one *reason to change*, not one method.
- **Making an abstraction for everything.** One implementation and no plan for more means you don't need a base class yet.
- **Inheriting just to reuse code.** Child classes should be a true "is-a" (a `Sparrow` is a `FlyingBird`). For reuse alone, use composition (a class that *holds* another).
- **Using `if isinstance(...)` chains.** Often a sign that O or L is being broken.
- **Hard-coding `SomeClass()` inside methods.** That's the D problem. Pass it in through `__init__`.
