# SOLID Principles in Python

SOLID principles are **5 important object-oriented programming (OOP) principles** that help us write clean, maintainable, reusable, and scalable code.

They are applicable to Python, Java, C++, and other object-oriented languages.

## SOLID Principles

1. **S - Single Responsibility Principle (SRP):** One class should have one responsibility.
2. **O - Open/Closed Principle (OCP):** Open for extension, closed for modification.
3. **L - Liskov Substitution Principle (LSP):** Child classes should be replaceable for parent classes.
4. **I - Interface Segregation Principle (ISP):** Don't force classes to implement methods they don't need.
5. **D - Dependency Inversion Principle (DIP):** Depend on abstractions, not concrete implementations.

---

## 1. S - Single Responsibility Principle (SRP)

**Definition:** A class should have only one reason to change.

Imagine you are developing an employee management application.

### Bad implementation

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def calculate_salary(self):
        return self.salary

    def save_to_database(self):
        print("Saving employee to database")

    def generate_report(self):
        print("Generating employee report")
```

What's the problem?

This class has three responsibilities:

1. Calculating salary.
2. Saving employee data.
3. Generating reports.

### Better implementation

```python
class Employee:
    def __init__(self, name, salary):
        self.name = name
        self.salary = salary

    def calculate_salary(self):
        return self.salary


class EmployeeDatabase:
    def save(self, employee):
        print("Saving employee to database")


class EmployeeReport:
    def generate(self, employee):
        print("Generating employee report")
```

Now each class has a separate responsibility.

**Remember:** One class = One primary responsibility.

---

## 2. O - Open/Closed Principle (OCP)

**Definition:** Software entities should be open for extension but closed for modification.

Let's imagine a payment application.

### Bad implementation

```python
class Payment:
    def pay(self, payment_type):
        if payment_type == "UPI":
            print("Paying through UPI")

        elif payment_type == "Card":
            print("Paying through Card")
```

Tomorrow, if you introduce Net Banking, you need to modify the existing class.

This becomes difficult as the application grows.

### Better implementation

```python
class Payment:
    def pay(self):
        pass


class UPIPayment(Payment):
    def pay(self):
        print("Paying through UPI")


class CardPayment(Payment):
    def pay(self):
        print("Paying through Card")


class NetBankingPayment(Payment):
    def pay(self):
        print("Paying through Net Banking")
```

### Usage

```python
payments = [
    UPIPayment(),
    CardPayment(),
    NetBankingPayment()
]

for payment in payments:
    payment.pay()
```

### Output

```text
Paying through UPI
Paying through Card
Paying through Net Banking
```

We can add new payment types without changing the existing payment implementations.

**Remember:** Add new functionality without unnecessarily modifying existing working code.

---

## 3. L - Liskov Substitution Principle (LSP)

**Definition:** Objects of a child class should be usable wherever objects of the parent class are expected, without breaking the expected behavior.

Let's understand with a real-world example.

Imagine a parent class called Bird.

### Bad implementation

```python
class Bird:
    def fly(self):
        print("Bird is flying")


class Sparrow(Bird):
    pass


class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins cannot fly")
```

Problem:

Penguin inherits from Bird, but it cannot satisfy the expected behavior of the `fly()` method.

### Better implementation

```python
class Bird:
    def eat(self):
        print("Bird is eating")


class FlyingBird(Bird):
    def fly(self):
        print("Bird is flying")


class Sparrow(FlyingBird):
    pass


class Penguin(Bird):
    pass
```

Now:

- Sparrow can eat and fly.
- Penguin can eat but doesn't need to fly.

We avoid forcing Penguin to implement behavior it cannot support.

**Remember:** A child class should behave consistently with what its parent promises.

---

## 4. I - Interface Segregation Principle (ISP)

**Definition:** A class should not be forced to implement methods it doesn't use.

Python doesn't have Java-style interfaces built into its syntax, but we can achieve this using abstract classes or Protocols.

Imagine a machine interface:

### Bad implementation

```python
class Machine:
    def print_document(self):
        pass

    def scan_document(self):
        pass

    def fax_document(self):
        pass


class SimplePrinter(Machine):
    def print_document(self):
        print("Printing")

    def scan_document(self):
        pass

    def fax_document(self):
        pass
```

Problem:

A simple printer only prints. Why should it implement scanning and faxing?

### Better implementation

```python
class Printer:
    def print_document(self):
        print("Printing")


class Scanner:
    def scan_document(self):
        print("Scanning")


class Fax:
    def fax_document(self):
        print("Faxing")


class SimplePrinter(Printer):
    pass


class MultiFunctionPrinter(Printer, Scanner, Fax):
    pass
```

Now each class implements only the functionality it needs.

**Remember:** Keep interfaces small and specific.

---

## 5. D - Dependency Inversion Principle (DIP)

**Definition:** High-level modules should not depend directly on low-level modules. Both should depend on abstractions.

This sounds complicated, but let's understand it.

Imagine you have an application that sends notifications.

### Bad implementation

```python
class Email:
    def send(self, message):
        print("Sending email:", message)


class Notification:
    def __init__(self):
        self.email = Email()

    def notify(self, message):
        self.email.send(message)
```

Problem:

The Notification class is tightly coupled to Email.

What if tomorrow you want SMS or WhatsApp notifications?

### Better implementation

```python
class Email:
    def send(self, message):
        print("Sending email:", message)


class SMS:
    def send(self, message):
        print("Sending SMS:", message)


class Notification:
    def __init__(self, service):
        self.service = service

    def notify(self, message):
        self.service.send(message)
```

### Usage

```python
email = Email()
sms = SMS()

notification1 = Notification(email)
notification1.notify("Hello!")

notification2 = Notification(sms)
notification2.notify("Hello!")
```

### Output

```text
Sending email: Hello!
Sending SMS: Hello!
```

Notice this line:

```python
notification = Notification(email)
```

We pass the dependency from outside instead of creating it inside the Notification class.

This is called **Dependency Injection**, a technique that helps implement Dependency Inversion.

**Remember:** Don't tightly couple your class to a particular implementation.

### Using abstraction:  
```python
from abc import ABC, abstractmethod


# Abstraction
class NotificationService(ABC):

    @abstractmethod
    def send(self, message):
        pass


# Concrete class 1
class Email(NotificationService):

    def send(self, message):
        print("Sending email:", message)


# Concrete class 2
class SMS(NotificationService):

    def send(self, message):
        print("Sending SMS:", message)


# High-level class
class Notification:

    def __init__(self, service):
        self.service = service

    def notify(self, message):
        self.service.send(message)


# Using Email
email = Email()
notification = Notification(email)
notification.notify("Hello!")

# Using SMS
sms = SMS()
notification = Notification(sms)
notification.notify("Hello!")  
```

---

# Quick Revision for Interviews

| Principle | Main idea | Remember |
|---|---|---|
| S - SRP | One responsibility | One job |
| O - OCP | Extend without modifying existing code | Extend |
| L - LSP | Child replaces parent safely | Substitution |
| I - ISP | Small, specific interfaces | Don't force |
| D - DIP | Depend on abstractions | Loose coupling |

---
