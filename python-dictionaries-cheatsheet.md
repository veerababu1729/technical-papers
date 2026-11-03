# Python Dictionary Cheat Sheet

---

## What is a dictionary?

Data stored as **key: value** pairs, like a phone book. You look up a name (key) and get a number (value).

- Written with `{ }`
- Mutable, so you can add, change and remove pairs
- Keys are unique. Values can repeat.
- Keys must be immutable (string, number, tuple). Values can be anything.
- Keeps insertion order (Python 3.7+)

```python
student = {"name": "Sam", "age": 21, "course": "python"}
```
- keys: `name`, `age`, `course`
- values: `Sam`, `21`, `python`

---

## Creating

```python
d = {"name": "Sam", "age": 21}      # literal
empty = {}                          # empty dict
empty2 = dict()                     # also empty
d2 = dict(name="Sam", age=21)       # keyword style
```  

Duplicate keys: the last one wins.
```python
{"a": 1, "a": 2}    # {'a': 2}
```

Note: `{}` is an empty dict, not an empty set. For an empty set, use `set()`.

---

## Accessing

**By key**
```python
person = {"name": "Sam", "company": "Acme", "age": 30}
print(person["name"])      # Sam
print(person["age"])       # 30
```

A missing key raises `KeyError`:
```python
print(person["city"])      # KeyError: 'city'
```

**With `get()`** (no error)
```python
print(person.get("city"))             # None
print(person.get("city", "unknown"))  # unknown
```

**Only keys**
```python
for key in person.keys():
    print(key)          # name, company, age
```

**Only values**
```python
for value in person.values():
    print(value)        # Sam, Acme, 30
```

**Both at once**
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
- `d.items()` gives (key, value) pairs in a tuple.

Looping over the dict directly gives keys:
```python
for key in person:
    print(key)
```

---

## Operations

### Modify a value
```python
d = {"phone": "X123", "year": 2021}
d["year"] = 2022
print(d)    # {'phone': 'X123', 'year': 2022}
```

### Add a new pair

Assign to a key that doesn't exist yet. Same syntax as modifying, and Python decides which one it is.
```python
d["color"] = "black"
print(d)    # {'phone': 'X123', 'year': 2022, 'color': 'black'}
```

### Update (add or overwrite many at once)
```python
d = {"a": 1}
d.update({"b": 2, "a": 10})
print(d)    # {'a': 10, 'b': 2}
```

### Remove a pair
```python
d = {"phone": "X123", "year": 2021, "color": "black"}

d.pop("year")           # removes it and returns 2021
print(d)                # {'phone': 'X123', 'color': 'black'}

del d["color"]          # removes it, returns nothing
print(d)                # {'phone': 'X123'}
```

**(extra)** More ways to remove:
```python
d.pop("missing")            # KeyError
d.pop("missing", None)      # None, no error

d = {"a": 1, "b": 2, "c": 3}
d.popitem()                 # ('c', 3), removes the last inserted pair

d.clear()                   # {}
del d                       # deletes the variable itself
```

### Check if a key exists
```python
d = {"device": "laptop", "year": 2020, "cost": 40000}

if "cost" in d:
    print("cost exists")

print("price" not in d)     # True
```

`in` checks **keys**. To check values:
```python
print("laptop" in d.values())   # True
```

### Length
```python
print(len(d))    # 3
```

---

## Methods table

| Method | What it does |
|--------|--------------|
| `clear()` | remove everything |
| `copy()` | shallow copy |
| `fromkeys(keys, val)` | new dict from keys, all with the same value |
| `get(key, default)` | value for key, or default if missing |
| `items()` | all (key, value) pairs |
| `keys()` | all keys |
| `values()` | all values |
| `pop(key)` | remove key and return its value |
| `popitem()` | remove and return the last inserted pair |
| `setdefault(key, val)` | return value; if key is missing, add it with val |
| `update(other)` | add or overwrite pairs from another dict |

The book says you don't need to memorise all of these for coding rounds. The ones you'll actually use: `get`, `items`, `keys`, `values`, `pop`, `update`, `setdefault`.

**(extra)** `setdefault` in action:
```python
d = {"name": "Sam"}
d.setdefault("city", "Pune")     # adds city, returns 'Pune'
d.setdefault("name", "Ravi")     # name already exists, nothing changes
print(d)    # {'name': 'Sam', 'city': 'Pune'}
```

---

## (extra) Nested dictionaries

A dict inside a dict.

```python
students = {
    "sam":   {"age": 21, "city": "Pune"},
    "priya": {"age": 22, "city": "Delhi"},
}

print(students["sam"]["age"])       # 21
students["priya"]["age"] = 23

for name, info in students.items():
    print(name, info["city"])
# sam Pune
# priya Delhi
```

---

## (extra) Dictionary comprehension

Build a dict in one line.

```python
squares = {n: n * n for n in range(1, 5)}
print(squares)    # {1: 1, 2: 4, 3: 9, 4: 16}

scores = {"sam": 80, "priya": 45, "ravi": 90}
passed = {k: v for k, v in scores.items() if v >= 50}
print(passed)     # {'sam': 80, 'ravi': 90}
```

---

## (extra) Merging

```python
a = {"x": 1, "y": 2}
b = {"y": 20, "z": 30}

print(a | b)         # {'x': 1, 'y': 20, 'z': 30}   (Python 3.9+)
print({**a, **b})    # same result, works on older versions too
```

On a clash, the right-hand dict wins.

---

## (extra) Copying

```python
a = {"x": 1}
b = a              # NOT a copy, both names point to the same dict
b["x"] = 99
print(a)           # {'x': 99}

c = a.copy()       # a real (shallow) copy
c["x"] = 5
print(a)           # {'x': 99}  unchanged
```

`copy()` is shallow. Nested dicts or lists inside are still shared. For a full independent copy:
```python
import copy
deep = copy.deepcopy(a)
```

---

## (extra) Sorting

A dict can't be sorted in place, but you can get a sorted view.

```python
scores = {"sam": 80, "priya": 45, "ravi": 90}

print(sorted(scores))                      # ['priya', 'ravi', 'sam']  (keys)

by_value = sorted(scores.items(), key=lambda kv: kv[1])
print(by_value)    # [('priya', 45), ('sam', 80), ('ravi', 90)]

top = sorted(scores.items(), key=lambda kv: kv[1], reverse=True)
print(top[0])      # ('ravi', 90)

print(dict(by_value))    # back to a dict, now in sorted order
```

---

## (extra) Patterns that show up in interviews

**Count how often each item appears**
```python
text = "banana"
count = {}
for ch in text:
    count[ch] = count.get(ch, 0) + 1
print(count)    # {'b': 1, 'a': 3, 'n': 2}
```

**Group items**
```python
words = ["apple", "avocado", "banana", "blueberry", "cherry"]
groups = {}
for w in words:
    groups.setdefault(w[0], []).append(w)
print(groups)
# {'a': ['apple', 'avocado'], 'b': ['banana', 'blueberry'], 'c': ['cherry']}
```

**Two Sum** (the dict makes it one pass)
```python
def two_sum(nums, target):
    seen = {}                        # value -> index
    for i, n in enumerate(nums):
        if target - n in seen:
            return [seen[target - n], i]
        seen[n] = i

print(two_sum([2, 7, 11, 15], 9))    # [0, 1]
```

**Swap keys and values** (only works if the values are unique and immutable)
```python
d = {"a": 1, "b": 2}
print({v: k for k, v in d.items()})    # {1: 'a', 2: 'b'}
```

---

## (extra) Gotchas

- **List as a key fails.** Lists are mutable, so they can't be keys. Use a tuple.
  ```python
  d = {[1, 2]: "x"}     # TypeError: unhashable type: 'list'
  d = {(1, 2): "x"}     # fine
  ```
- **Don't change the dict's size while looping over it.** Loop over a copy.
  ```python
  for k in list(d):
      if d[k] < 50:
          del d[k]
  ```
- **`d[key]` vs `d.get(key)`.** The first raises `KeyError`, the second gives `None`.
- **`1`, `1.0` and `True` are the same key**, since they're equal.
  ```python
  {1: "a", True: "b"}    # {1: 'b'}
  ```
- **`fromkeys` with a list value shares one list** across all keys. Use a comprehension instead:
  ```python
  {k: [] for k in ["a", "b"]}
  ```
**- **Speed.** Looking up, adding or deleting a key is O(1) on average. `x in d` is fast, but `x in d.values()` scans every value (O(n)).**
