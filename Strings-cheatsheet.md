# Python Strings Cheat Sheet

Strings is on of the most imp topic in python.

---

## What is a string?

Text inside single or double quotes. A word, a sentence, even digits.

```python
s1 = 'hello'
s2 = "hello"        # same thing
s3 = "Hello World!"
print(s3)           # Hello World!
```

**(extra)** Quote tricks:
```python
a = "It's fine"                  # single quote inside double quotes
b = 'She said "hi"'              # double quotes inside single quotes
c = "line one\nline two"         # \n = new line
d = "name:\tSam"                 # \t = tab
e = """This text
spans many
lines"""                         # triple quotes for multi-line
```

---

## Accessing characters

Every character has an index. Counting starts at 0, or from -1 at the end.

```
 p   y   t   h   o   n
 0   1   2   3   4   5
-6  -5  -4  -3  -2  -1
```

```python
s = "python"
print(s[0])      # p
print(s[2])      # t
print(s[-1])     # n
print(s[-2])     # o
```

Loop through it:
```python
for ch in "python":
    print(ch)
# p
# y
# t
# h
# o
# n
```

An index past the end raises `IndexError`.

---

## Slicing

Take a part of a string, like cutting a slice from a pizza.

```python
string[start : end : step]
```
- `start`: where to begin (included)
- `end`: where to stop (**not** included)
- `step`: how many to jump (optional)

```
 L  o  v  e     P  y  t  h  o  n
 0  1  2  3  4  5  6  7  8  9  10
```

```python
s = "Love Python"

print(s[2:8])        # ve Pyt
print(s[2:8:2])      # v y         (indexes 2, 4, 6)
print(s[0:10:2])     # Lv yh       (indexes 0, 2, 4, 6, 8)
print(s[:4])         # Love        (from the start)
print(s[2:])         # ve Python   (to the end)
print(s[-5:-2])      # yth
print(s[::-1])       # nohtyP evoL  (reversed)
print(s[-2:-5:-1])   # oht         (negative step goes backwards)
```

Leave a part empty and Python fills in the default: start = 0, end = the end, step = 1.

### Reverse a string

```python
s1 = "Practice makes perfect"
print(s1[::-1])      # tcefrep sekam ecitcarP
```

Slicing never raises an error for out-of-range values. `"abc"[1:100]` just gives `bc`.

---

## Strings are immutable

You can't change a character in place.

```python
s = "python"
s[2] = "k"
# TypeError: 'str' object does not support item assignment
```

To "change" a string, build a new one:
```python
s = "python"
s = s[:2] + "k" + s[3:]
print(s)    # pykhon
```

Every string method returns a **new** string. The original stays as it was.

---

## Basic operations

**Concatenation (+)**
```python
s1 = "sun"
s2 = "flower"
print(s1 + s2)          # sunflower
print(s1 + " " + s2)    # sun flower
```
`+` doesn't add spaces for you. You can only join string with string. `"age: " + 5` fails, so use `"age: " + str(5)`.

**Repetition (\*)**
```python
s1 = "Ha"
print(s1 * 3)    # HaHaHa
```

**Membership (in)**
```python
s = "python"
print("y" in s)         # True
print("thon" in s)      # True  (works for parts, not only single letters)
print("z" not in s)     # True
```

**Length (len)**
```python
s1 = "sunflower"
print(len(s1))    # 9
```
Spaces count as characters.

---

## Built-in methods

Highly important in coding. Dot notation: `s.method()`.

### Case changing

```python
s = "hello world"

print(s.upper())        # HELLO WORLD
print(s.lower())        # hello world
print(s.title())        # Hello World    (first letter of each word)
print(s.capitalize())   # Hello world    (first letter of the sentence)
print(s.swapcase())     # HELLO WORLD    (flips every letter's case)
```

### Trimming (remove spaces)

```python
s = "  hello  "

print(s.strip())     # "hello"      both sides
print(s.lstrip())    # "hello  "    left side
print(s.rstrip())    # "  hello"    right side
```

`strip()` only removes the edges, never the spaces in the middle.

### Padding (fill up to a width)

```python
s = "python"

print(s.center(10, "-"))    # --python--
print(s.ljust(10, "-"))     # python----     (text on the left)
print(s.rjust(10, "-"))     # ----python     (text on the right)
```

Arguments are (total width, fill character). The default fill is a space.

### Search and replace

```python
s = "hello world, welcome to the world"

print(s.find("world"))                # 6    first match
print(s.rfind("world"))               # 28   last match
print(s.index("world"))               # 6    same as find, but errors if missing
print(s.replace("world", "python"))   # hello python, welcome to the python
```

- `find()` returns `-1` if the text isn't found.
- `index()` raises `ValueError` if the text isn't found.
- `replace()` replaces **every** match. To limit it: `s.replace("world", "python", 1)`.

### Splitting and joining

**split()** turns a string into a list.
```python
s = "red green blue"
print(s.split(" "))        # ['red', 'green', 'blue']

s = "apple,banana,grape"
print(s.split(","))        # ['apple', 'banana', 'grape']
```
With no argument, `split()` splits on any whitespace.

**join()** turns a list back into a string. The separator goes first.
```python
words = ["Hello", "world"]
print(" ".join(words))     # Hello world
print("-".join(words))     # Hello-world
print("".join(words))      # Helloworld
```
The list must contain only strings.

---

## (extra) More methods you'll use

**Check what's inside**
```python
s = "python"
print(s.startswith("py"))     # True
print(s.endswith("on"))       # True
print(s.count("t"))           # 1

print("123".isdigit())        # True
print("abc".isalpha())        # True
print("abc123".isalnum())     # True
print("   ".isspace())        # True
print("hello".islower())      # True
print("HELLO".isupper())      # True
```

**Character codes**
```python
print(ord("a"))    # 97
print(chr(97))     # a
```

**Comparing**
```python
print("apple" == "apple")    # True
print("apple" == "Apple")    # False, case matters
print("apple" < "banana")    # True, compared alphabetically
```

**Formatting with f-strings**
```python
name = "Priya"
score = 92.456
print(f"{name} scored {score:.1f}")    # Priya scored 92.5
```

**Strings and numbers**
```python
int("42")       # 42
str(42)         # '42'
list("abc")     # ['a', 'b', 'c']
```

---

## (extra) Methods at a glance

| Method | What it does |
|--------|--------------|
| `upper()` / `lower()` | all caps / all small |
| `title()` | Each Word Capitalised |
| `capitalize()` | First letter capital |
| `swapcase()` | flip every letter's case |
| `strip()` / `lstrip()` / `rstrip()` | remove edge spaces |
| `center(w, c)` / `ljust(w, c)` / `rjust(w, c)` | pad to width w with c |
| `find(x)` / `rfind(x)` | first / last position, -1 if missing |
| `index(x)` | position, error if missing |
| `replace(old, new)` | replace all matches |
| `split(sep)` | string to list |
| `sep.join(list)` | list to string |
| `startswith(x)` / `endswith(x)` | True or False |
| `count(x)` | how many times x appears |
| `isdigit()` / `isalpha()` / `isalnum()` | check the characters |

---

## (extra) Patterns that show up in interviews

**Palindrome check**
```python
def is_palindrome(s):
    s = s.lower()
    return s == s[::-1]

print(is_palindrome("Level"))    # True
print(is_palindrome("Python"))   # False
```

**Count vowels**
```python
s = "Programming"
count = 0
for ch in s.lower():
    if ch in "aeiou":
        count += 1
print(count)    # 3
```

**Reverse the order of words**
```python
s = "I love python"
print(" ".join(s.split()[::-1]))    # python love I
```

**Count how often each character appears**
```python
s = "banana"
freq = {}
for ch in s:
    freq[ch] = freq.get(ch, 0) + 1
print(freq)    # {'b': 1, 'a': 3, 'n': 2}
```

**Anagram check** (same letters, different order)
```python
print(sorted("listen") == sorted("silent"))    # True
```

**Remove duplicate characters, keep order**
```python
s = "programming"
result = ""
for ch in s:
    if ch not in result:
        result += ch
print(result)    # progamin
```

---

## Common mistakes

- Trying `s[0] = "x"`. Strings can't be changed in place.
- Forgetting that `s.upper()` doesn't change `s`. Write `s = s.upper()`.
- Using `+` with a number: `"age: " + 5` fails. Use `str(5)` or an f-string.
- Using `index()` when the text might be missing, which crashes. `find()` is safer.
- Expecting the end index in a slice to be included. `s[0:3]` gives 3 characters, not 4.
- Forgetting case: `"Python" == "python"` is `False`.
