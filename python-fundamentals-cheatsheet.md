# Python Fundamentals Cheat Sheet

This cheat sheet, completely prepared from my own notes, covers Day 1 to Day 5. 

---

## Day 1: First Program

```python
print("Hello World")
```
Output: `Hello World`

```python
a = 10
b = 20
print(a + b)
```
Output: `30`

Where to run it: VS Code, Python IDLE, or any online compiler.

---  
<br>
<br>
<br>
<br>

## Day 2: The Basics

### Keywords

Reserved words. You can't use them as variable names.

```
if  else  elif  for  while  def  return  import  from  as  in
and  or  not  try  except  finally  with  class  lambda
```

To see the full list:
```python
import keyword
print(keyword.kwlist)
```

### Comments

Python ignores them. They're only there for humans.

```python
# this is a single line comment
print(10 + 20)   # sum of two numbers
```

### Variables

A variable is a labelled box. The value is what you put inside.

```python
name = "Ravi Kumar"
age = 31
is_student = False
```

Naming rules:
- Start with a letter or underscore: `name`, `_name`
- Can't start with a number: `7name` is wrong
- Only letters, digits, underscores: `name_2` is fine, `my-name` and `@name` are not
- Case sensitive: `age` and `Age` are different variables

### Data Types

| Type | Example |
|------|---------|
| int | `age = 25` |
| float | `height = 5.9` |
| str | `name = "Ravi"` |
| bool | `is_student = True` |
| list | `colors = ["red", "blue", "green"]` |
| tuple | `point = (10, 20)` |
| dict | `person = {"name": "John", "age": 25}` |
| set | `ids = {512, 513, 514}` |
| NoneType | `x = None` |

Check a type with `type(x)`.

### Type Casting

Converting one type to another.

```python
int(2.8)        # 2   (cuts the decimal, doesn't round)
int("3")        # 3
float(1)        # 1.0
float("8.6")    # 8.6
str(2)          # '2'
str(3.0)        # '3.0'
```

`int("3.5")` throws an error. Go through `float` first: `int(float("3.5"))`.

### Input

`input()` always gives you a string.

```python
s = input("Enter a sentence: ")
print("You typed:", s)
```

For numbers, wrap it:

```python
n = int(input("Enter a number: "))
price = float(input("Enter a price: "))
```

### Output

Strings:
```python
s1 = "Masala chai"
print(s1)                        # Masala chai
print("Sam ordered " + s1)       # Sam ordered Masala chai
print(f"Sam ordered {s1}")       # Sam ordered Masala chai
```

Joining with `+` doesn't add spaces, so add them yourself:
```python
first = "Good"
second = "morning"
print(first + " " + second)      # Good morning
```

f-strings (put `f` before the quotes, variables go in `{}`):
```python
name = "Priya"
age = 23
print(f"My name is {name} and I am {age} years old")
```

Numbers:
```python
a, b, c = 10, 20, 30
print(a, b, c)                          # 10 20 30
print("numbers are:", a, b, c)          # numbers are: 10 20 30
print(f"first: {a}, second: {b}, third: {c}")
```

Control the separator and the line ending:
```python
print(a, b, c, sep=", ")     # 10, 20, 30
print(a, end=" ")            # stays on the same line
```

Decimal places:
```python
x = 123.456789
print(f"{x:.2f}")    # 123.46
print(f"{x:.3f}")    # 123.457
```

---
<br>
<br>
<br>
<br>

## Day 3: Operators

### Arithmetic

With `a = 10`, `b = 3`:

| Operator | Meaning | Result |
|----------|---------|--------|
| `+` | add | `a + b` → 13 |
| `-` | subtract | `a - b` → 7 |
| `*` | multiply | `a * b` → 30 |
| `/` | divide | `a / b` → 3.333... |
| `%` | remainder | `a % b` → 1 |
| `**` | power | `a ** b` → 1000 |
| `//` | floor division (drops decimal) | `a // b` → 3 |

### Assignment

With `m = 15`:

| Operator | Same as | m becomes |
|----------|---------|-----------|
| `m += 10` | `m = m + 10` | 25 |
| `m -= 10` | `m = m - 10` | 5 |
| `m *= 10` | `m = m * 10` | 150 |
| `m /= 10` | `m = m / 10` | 1.5 |
| `m %= 10` | `m = m % 10` | 5 |
| `m **= 2` | `m = m ** 2` | 225 |
| `m //= 10` | `m = m // 10` | 1 |

### Comparison

Always gives `True` or `False`.

```python
x = 10
y = 20
print(x == y)    # False  (equal)
print(x != y)    # True   (not equal)
print(x > y)     # False
print(x >= y)    # False
print(x < y)     # True
print(x <= y)    # True
```

`=` assigns a value. `==` compares two values.

### Logical

```python
a, b, c = 40, 20, 10

if (a > b) and (a > c):
    print("a is the greatest")       # both must be true

if (5 > 3) or (5 > 14):
    print("at least one is true")    # one is enough

print(not (2 < 5))                   # False, flips the result
```

### Membership

Checks if a value is inside a list, string, etc.

```python
fruits = ["apple", "banana"]
print("banana" in fruits)            # True
print("pineapple" not in fruits)     # True
```

---
<br>
<br>
<br>
<br>

## Day 4: Conditions and Loops

### Conditional Statements

Used to make decisions. Picture a fork in the road: left goes to the library, right goes to the cafe. Which way you go depends on what you want.

Indentation matters. The indented lines are what belong to the `if`.

**if**
```python
if 20 > 10:
    print("20 is greater")
```

**if-else**
```python
n = 7
if n % 2 == 0:
    print("even")
else:
    print("odd")
```

**if-elif-else**
```python
a, b, c = 10, 30, 20

if a > b and a > c:
    print("a is largest")
elif b > c:
    print("b is largest")
else:
    print("c is largest")
```
Output: `b is largest`

Only the first true branch runs. The rest are skipped.

### Loops

A loop repeats a task so you don't write the same line again and again. Printing 1 to 10,000 with separate `print` lines is silly. Use a loop.

#### for loop

```python
for i in range(start, stop, step):
    # statements
```

`stop` is **not** included. `start` defaults to 0 and `step` defaults to 1.

```python
for i in range(0, 11):        # 0 to 10
    print(i)

for i in range(0, 11, 2):     # 0 2 4 6 8 10
    print(i)

for i in range(10, 0, -1):    # 10 down to 1
    print(i)
```

All on one line:
```python
for i in range(0, 11):
    print(i, end=" ")
# 0 1 2 3 4 5 6 7 8 9 10
```

Changing `i` inside a `for` loop doesn't affect the next round. `range` hands out the next value no matter what.

#### while loop

Repeats as long as the condition is true. You have to update the variable yourself, or it runs forever.

```python
i = 0               # start
while i < 11:       # condition
    print(i)
    i = i + 1       # update
```

Print 1 to 20:
```python
i = 1
while i <= 20:
    print(i, end=" ")
    i = i + 1
```

Print 20 down to 1:
```python
i = 20
while i >= 1:
    print(i, end=" ")
    i = i - 1
```

Even numbers up to 10:
```python
i = 0
while i <= 10:
    print(i, end=" ")
    i = i + 2
```
Output: `0 2 4 6 8 10`

Numbers divisible by 3 from 1 to 20:
```python
i = 1
while i <= 20:
    if i % 3 == 0:
        print(i, end=" ")
    i += 1
```
Output: `3 6 9 12 15 18`

Use `for` when you know how many times to repeat. Use `while` when you're repeating until something changes.

---
<br>
<br>
<br>
<br>

## Day 5: Functions

A function is a block of code that does one specific job. You write it once and use it as many times as you want.

Python has two kinds: user-defined and built-in.

### User-defined Functions

Say you need the area of three rectangles. Copy-pasting this works, but it's messy:

```python
l1 = 10
b1 = 20
area = l1 * b1
print("Area of rectangle 1:", area)

l2 = 12
b2 = 2
area = l2 * b2
print("Area of rectangle 2:", area)
# ...and so on
```

With a function:

```python
def area(l, b):
    return l * b

print(area(10, 20))   # 200
print(area(12, 2))    # 24
print(area(5, 4))     # 20
```

**Step 1: declare it**
```python
def function_name(parameters):
    # statements
    return value
```

**Step 2: call it**
```python
def area_calc(l, b):
    area = l * b
    return area

c = area_calc(2, 10)
print("Area of rectangle:", c)    # Area of rectangle: 20
```

What happens on the call line:
- Python jumps into the function with `l = 2` and `b = 10`
- It works out `2 * 10 = 20`
- `return` sends 20 back to where the function was called
- That 20 is stored in `c`

It's like borrowing sugar from a neighbour. You ask (call), they hand it over (return), and you carry on.

A function doesn't have to return anything:
```python
def add(a, b):
    c = a + b
    print("sum is:", c)

add(20, 2)    # sum is: 22
```
Most of the time you'll use `return`, because it lets you reuse the result.

### Built-in Functions

Ready-made functions that Python already ships with. Say you want the length of a string. You could count it by hand:

```python
s = input("Enter some string: ")
count = 0
for ch in s:
    count = count + 1
print("String length:", count)
```

Or just use `len`:

```python
s = input("Enter some string: ")
print("String length:", len(s))
```

Common ones:

```python
a = -10
b = 3.768
c = [1, 2, 3, 4, 5]

print(abs(a))        # 10     absolute value
print(round(b))      # 4      nearest integer
print(round(b, 2))   # 3.77   to 2 decimal places
print(pow(2, 3))     # 8      2 to the power 3
print(max(c))        # 5
print(min(c))        # 1
print(sum(c))        # 15
print(len(c))        # 5
```

Random number (needs an import):
```python
import random
n = random.randint(1, 10)   # anything from 1 to 10, both included
print(n)
```
