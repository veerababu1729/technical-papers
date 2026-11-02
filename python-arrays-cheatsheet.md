# Python Arrays Cheat Sheet

Lists, Tuples, Sets, Dictionaries.

---

## Lists

A list stores multiple values in one variable. Think of a grocery list: one sheet, many items. The items can be of different types.

Lists are ordered, changeable (mutable), and allow duplicates.

### Creating

Square brackets, items separated by commas.

```python
fruits = ["apple", "banana", "cherry"]
numbers = [1, 2, 3, 4, 5]
mixed = ["hello", 42, 3.5, True]
empty = []
```

### Accessing

**Positive index** (starts from 0)
```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])    # apple
print(fruits[2])    # cherry
```

**Negative index** (starts from -1, counts from the end)
```python
print(fruits[-1])   # cherry
print(fruits[-2])   # banana
```

```
index:    0        1         2
        apple   banana   cherry
index:   -3       -2        -1
```

**Slicing** `list[start:stop]`

The stop index is not included.
```python
print(fruits[0:2])   # ['apple', 'banana']
print(fruits[:2])    # ['apple', 'banana']
print(fruits[1:])    # ['banana', 'cherry']
print(fruits[-2:])   # ['banana', 'cherry']
```

**Loops**
```python
# 1. by index
for i in range(len(fruits)):
    print(fruits[i])

# 2. by value
for fruit in fruits:
    print(fruit)

# 3. index and value together
for index, fruit in enumerate(fruits):
    print(index, fruit)
# 0 apple
# 1 banana
# 2 cherry
```

`i` is just a variable name. Call it anything. People use `i` out of habit.

### Operations

**Modify.** Lists are mutable, so you can change items in place.
```python
nums = [10, 20, 30, 40, 50]
nums[1] = 25
print(nums)          # [10, 25, 30, 40, 50]
```
(Strings are immutable. Lists are not.)

**Add**
```python
nums.append(60)          # adds at the end
# [10, 25, 30, 40, 50, 60]

nums.insert(2, 15)       # insert(index, value)
# [10, 25, 15, 30, 40, 50, 60]

nums.extend([70, 80, 90])   # merge another list in
# [10, 25, 15, 30, 40, 50, 60, 70, 80, 90]
```

**Remove**
```python
nums.remove(30)     # by value, first match only
# [10, 25, 15, 40, 50, 60, 70, 80, 90]

nums.pop(2)         # by index, removes 15
nums.pop()          # no index = removes the last item (90)
# [10, 25, 40, 50, 60, 70, 80]

del nums[2]         # delete by index, removes 40
# [10, 25, 50, 60, 70, 80]

nums.clear()        # empties the list
# []
```

`del nums` (without an index) deletes the whole variable. Printing it afterwards gives an error.

**Check if an item exists**
```python
nums = [10, 20, 30, 40]
if 30 in nums:
    print("30 is in the list")
```

**len, max, min**
```python
print(len(nums))    # 4
print(max(nums))    # 40
print(min(nums))    # 10
```

**Sorting**
```python
L = [10, 30, 20, 40, 50]

L.sort()                  # ascending, changes the original
# [10, 20, 30, 40, 50]

L.sort(reverse=True)      # descending
# [50, 40, 30, 20, 10]
```

`sorted()` gives you a new list and leaves the original alone:
```python
L1 = [10, 60, 30]
L2 = sorted(L1)
print(L2)    # [10, 30, 60]
print(L1)    # [10, 60, 30]  unchanged
```

`.sort()` returns `None`. So `x = L.sort()` leaves `x` empty. Use `sorted()` if you want a value back.

**Reverse and index**
```python
L = [10, 20, 30]
L.reverse()          # [30, 20, 10]
print(L.index(20))   # 1
```

**Nested list** (a list inside a list)
```python
m = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
print(m[1])       # [4, 5, 6]
print(m[1][2])    # 6   (row 1, item 2)
```

**Count**
```python
L = [1, 1, 20, 30, 40, 1]
print(L.count(1))    # 3
```

### List methods at a glance

| Method | What it does |
|--------|--------------|
| `append(x)` | add x at the end |
| `insert(i, x)` | add x at index i |
| `l1.extend(l2)` | merge l2 into l1 |
| `remove(x)` | remove first x |
| `pop(i)` | remove and return item at i |
| `pop()` | remove and return the last item |
| `clear()` | remove everything |
| `index(x)` | position of x |
| `count(x)` | how many times x appears |
| `sort()` | sort in place |
| `reverse()` | reverse in place |
| `sorted(l)` | new sorted list, original unchanged |
| `len(l)`, `max(l)`, `min(l)` | size, biggest, smallest |
| `x in l` | True if x exists |

---

## Tuples

Like a list, but **immutable**. Once created, you can't change, add or remove items.

Ordered, unchangeable, allows duplicates.

### Creating

Round brackets.
```python
langs = ("python", "java", "c")
print(langs)    # ('python', 'java', 'c')
```

A tuple with one item needs a trailing comma:
```python
single = (5,)     # tuple
not_tuple = (5)   # just the number 5
```

### Unchangeable

```python
langs[1] = "go"
# TypeError: 'tuple' object does not support item assignment
```
`append()`, `remove()` and the like don't exist on tuples.

### Accessing

```python
langs = ("python", "java", "c")
print(langs[0])    # python
print(langs[2])    # c

for lang in langs:
    print(lang)
```

### Operations

```python
print(len(langs))            # 3

if "java" in langs:
    print("java is there")

print(langs[0:2])            # ('python', 'java')

print(langs.count("c"))      # 1
print(langs.index("c"))      # 2
```

### Changing a tuple (the workaround)

Convert to a list, change it, convert back.
```python
langs = ("python", "java", "c")
temp = list(langs)
temp[1] = "go"
temp.append("rust")
langs = tuple(temp)
print(langs)    # ('python', 'go', 'c', 'rust')
```

---

## Sets

A collection with **no duplicates**. Unordered, so no indexes.

Written with `{ }` or `set()`.

### Creating

```python
n1 = {1, 2, 3}
n2 = set([1, 2, 3])
print(n1)    # {1, 2, 3}
```

The order can change every time you run it. Sets don't keep order.

An empty set must be `set()`. `{}` gives you an empty dictionary.

### No duplicates

```python
numbers = {1, 2, 2, 3, 4, 4}
print(numbers)    # {1, 2, 3, 4}
```

### Accessing

No indexing:
```python
numbers = {1, 2, 3, 4}
print(numbers[2])
# TypeError: 'set' object is not subscriptable
```

Loop through it instead:
```python
for n in numbers:
    print(n)
```

### Operations

**Add**
```python
numbers = {1, 2, 3}
numbers.add(4)
numbers.add((5, 6))      # a tuple counts as one item
print(numbers)           # {1, 2, 3, 4, (5, 6)}
```

**Add two sets**
```python
s1 = {1, 2, 3}
s2 = {4, 5, 6}
s1.update(s2)
print(s1)    # {1, 2, 3, 4, 5, 6}
```

**Remove**
```python
numbers = {1, 2, 3, 4}
numbers.remove(2)
print(numbers)    # {1, 3, 4}
```
`remove()` raises an error if the item isn't there. `discard()` does the same job quietly.

**Clear**
```python
numbers.clear()
print(numbers)    # set()
```

### Set operations

```python
a = {1, 2, 3}
b = {3, 4, 5}

print(a.union(b))                 # {1, 2, 3, 4, 5}   everything
print(a.intersection(b))          # {3}               common items
print(a.difference(b))            # {1, 2}            only in a
print(a.symmetric_difference(b))  # {1, 2, 4, 5}      not common
```

**Check, length, convert to list**
```python
numbers = {10, 20, 40}

if 30 in numbers:
    print("30 exists")
else:
    print("30 is not there")

print(len(numbers))        # 3

nlist = list(numbers)      # now you can index it
```

### Only immutable items allowed

Numbers, strings and tuples are fine. Lists, dictionaries and sets are not.

```python
myset = {1, 2, [3, 4]}     # error, list not allowed
myset = {1, 2, (3, 4)}     # fine, tuple is allowed
```

---

## Dictionaries

Data stored as **key: value** pairs, like a phone book (name is the key, number is the value).

Written with `{ }`. Mutable. Keys must be unique and immutable (strings and numbers are the usual choice).

Modern Python keeps insertion order in dicts, even though the book calls them unordered.

### Creating

```python
student = {"name": "Sam", "age": 21, "course": "python"}
```
- keys: `name`, `age`, `course`
- values: `Sam`, `21`, `python`

### Accessing

**By key**
```python
person = {"name": "Sam", "company": "Acme", "age": 30}
print(person["name"])       # Sam
print(person["age"])        # 30
```
A missing key raises `KeyError`. `person.get("city")` returns `None` instead.

**Only keys**
```python
for key in person.keys():
    print(key)        # name, company, age
```

**Only values**
```python
for value in person.values():
    print(value)      # Sam, Acme, 30
```

**Both together**
```python
for key, value in person.items():
    print(key, value)
# name Sam
# company Acme
# age 30
```

Shortcut:
- `d.keys()` gives all keys
- `d.values()` gives all values
- `d.items()` gives (key, value) pairs

### Operations

**Modify a value**
```python
d = {"phone": "X123", "year": 2021}
d["year"] = 2022
print(d)    # {'phone': 'X123', 'year': 2022}
```

**Add a new pair**

Assign to a key that doesn't exist yet.
```python
d["color"] = "black"
print(d)    # {'phone': 'X123', 'year': 2022, 'color': 'black'}
```

**Remove a pair**
```python
d = {"phone": "X123", "year": 2021}

d.pop("year")
print(d)    # {'phone': 'X123'}

del d["phone"]
print(d)    # {}
```

**Check if a key exists**
```python
d = {"device": "laptop", "year": 2020, "cost": 40000}
if "cost" in d:
    print("cost exists")
```
`in` checks keys, not values.

**Length**
```python
print(len(d))    # 3
```

### Dictionary methods

The book says you don't need to memorise these for coding rounds. Know `get`, `items`, `keys`, `values`, `pop` and `update`.

| Method | What it does |
|--------|--------------|
| `clear()` | remove everything |
| `copy()` | make a copy |
| `fromkeys()` | build a dict from given keys and one value |
| `get(key)` | value for key (None if missing) |
| `items()` | all (key, value) pairs |
| `keys()` | all keys |
| `values()` | all values |
| `pop(key)` | remove that key |
| `popitem()` | remove the last inserted pair |
| `setdefault(key, val)` | return value; if key is missing, add it with val |
| `update({...})` | add or overwrite pairs from another dict |

```python
d = {"a": 1}
d.update({"b": 2, "a": 10})
print(d)    # {'a': 10, 'b': 2}
```

---

## Quick comparison

| | List | Tuple | Set | Dict |
|---|------|-------|-----|------|
| Brackets | `[ ]` | `( )` | `{ }` | `{k: v}` |
| Ordered | yes | yes | no | yes (keys) |
| Changeable | yes | no | yes | yes |
| Duplicates | yes | yes | no | keys: no |
| Indexing | yes | yes | no | by key |
