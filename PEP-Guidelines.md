# PEP 8 Cheat Sheet.

Source: https://peps.python.org/pep-0008/

---

## The idea

Code is read far more often than it's written. PEP 8 is about readability and consistency.

Priority when rules clash:
1. Consistency inside one function or module (most important)
2. Consistency inside your project
3. Consistency with PEP 8

If your team has its own style guide, follow that. Don't rewrite old code only to satisfy PEP 8.

---

## Top 10 for real work

1. Indent with **4 spaces**, never tabs.
2. Keep lines to **79 characters** (many teams use 88 to 120).
3. Variables and functions: `snake_case`.
4. Classes: `CapWords`.
5. Constants: `UPPER_CASE`.
6. Imports at the top, one per line, in 3 groups, no `import *`.
7. Compare to `None` with `is` / `is not`, never `==`.
8. Don't use a bare `except:`. Catch a specific exception.
9. Use `if items:` instead of `if len(items):`, and `if flag:` instead of `if flag == True:`.
10. Comments explain **why**, not what. Public functions get docstrings.

---

## Code layout

### Indentation (daily)

4 spaces per level. Spaces are preferred. Python 3 won't let you mix tabs and spaces.

Wrapped lines: align with the opening bracket, or use a hanging indent.

```python
# Good: aligned with the opening bracket
foo = long_function_name(var_one, var_two,
                         var_three, var_four)

# Good: hanging indent, nothing on the first line
foo = long_function_name(
    var_one, var_two,
    var_three, var_four)

# Good: extra indent so arguments don't look like the body
def long_function_name(
        var_one, var_two, var_three,
        var_four):
    print(var_one)
```

```python
# Bad: arguments on the first line AND no alignment
foo = long_function_name(var_one, var_two,
    var_three, var_four)

# Bad: arguments look like the function body
def long_function_name(
    var_one, var_two, var_three,
    var_four):
    print(var_one)
```

Closing bracket on a multi-line construct: either lines up with the first character of the line that opened it (most common) or under the first item.

```python
my_list = [
    1, 2, 3,
    4, 5, 6,
]
```

### Line length (daily)

- Code: **79** characters
- Comments and docstrings: **72**
- Teams may agree on up to **99**, but comments and docstrings stay at 72

Break long lines inside `()`, `[]`, `{}` instead of using a backslash `\`.

```python
total = (first_value
         + second_value
         - discount)
```

PEP 8 prefers breaking **before** a binary operator (`+`, `-`, `and`, `or`), as above. Breaking after is allowed if the file is consistent.

### Blank lines (daily)

- **2** blank lines around top-level functions and classes
- **1** blank line between methods inside a class
- Use blank lines inside functions sparingly, to mark logical sections

```python
import os


def first():
    ...


def second():
    ...


class Thing:
    def a(self):
        ...

    def b(self):
        ...
```

### File encoding

UTF-8. No encoding declaration needed in Python 3.

### Imports (daily)

- One import per line. `from x import a, b` is fine.
- At the top of the file, after the module docstring and before globals or constants.
- Three groups, a blank line between each:
  1. Standard library
  2. Third-party
  3. Your own project
- Prefer absolute imports. Explicit relative imports (`from . import x`) are acceptable inside packages.
- Avoid `from module import *`. It hides where names come from.

```python
import os
import sys

import requests

from myapp import utils
```

```python
# Bad
import sys, os
from os import *
```

### Module-level dunders

`__all__`, `__version__`, `__author__` go after the module docstring and before imports (except `from __future__` imports, which come first).

---

## String quotes

Single or double quotes. PEP 8 doesn't pick one, so choose one and stay consistent.

- If the string contains one kind of quote, use the other kind to avoid backslashes: `"it's"` or `'say "hi"'`.
- Triple-quoted strings use `"""` (matches the docstring convention).

---

## Whitespace

### Avoid extra spaces (daily)

```python
# Good                              # Bad
spam(ham[1], {eggs: 2})             spam( ham[ 1 ], { eggs: 2 } )
foo = (0,)                          foo = (0, )
if x == 4: print(x, y)              if x == 4 : print(x , y)
spam(1)                             spam (1)
d['key'] = lst[index]               d ['key'] = lst [index]
x = 1                               x             = 1
long_name = 3                       long_name     = 3
```

No trailing whitespace at the end of lines.

### Operators (daily)

One space on each side of: `=`, `+=`, `-=`, comparisons (`==`, `<`, `>`, `!=`, `in`, `is`), and `and` / `or` / `not`.

```python
i = i + 1
submitted += 1
x = x*2 - 1                  # tighter spacing for the higher-priority operator is allowed
hypot2 = x*x + y*y
c = (a+b) * (a-b)
```

Never use more than one space, and keep the same amount on both sides.

### Keyword arguments and defaults (daily)

No spaces around `=` for keyword arguments or defaults of unannotated parameters:

```python
def complex(real, imag=0.0):
    return magic(r=real, i=imag)
```

With a type annotation, use spaces around `=`:

```python
def munge(sep: str = None): ...
```

### Slices

The colon acts like an operator. Keep the spacing equal on both sides, and drop the space when a part is omitted:

```python
ham[1:9]
ham[1:9:3]
ham[lower + offset : upper + offset]
ham[:9:3]
```

### One statement per line

```python
# Good
if foo == 'blah':
    do_blah_thing()
do_one()
do_two()

# Bad
if foo == 'blah': do_blah_thing()
do_one(); do_two()
```

---

## Trailing commas

- Needed for a one-item tuple: `FILES = ('setup.cfg',)`
- Handy in multi-line lists, calls and imports. Each item on its own line with a trailing comma means Git diffs only show the new line.

```python
FILES = [
    'setup.cfg',
    'tox.ini',
]
```

Not on a single line: `['a', 'b',]` is wrong.

---

## Comments

A comment that contradicts the code is worse than no comment. Keep them current.

- Write complete sentences, in English.
- Block comments: start with `# `, indented to match the code below.
- Inline comments: sparingly, at least **2 spaces** after the code, then `# `.
- Don't state the obvious.

```python
x = x + 1                 # Increment x           <- useless
x = x + 1                 # Compensate for border <- explains why
```

### Docstrings (daily)

Write them for every public module, function, class and method (full rules are in PEP 257).

```python
def area(length, width):
    """Return the area of a rectangle."""
    return length * width


def build_report(data, fmt="pdf"):
    """Build a report from the given data.

    fmt is the output format, either "pdf" or "csv".
    """
```

- One-liner: closing `"""` on the same line.
- Multi-line: closing `"""` on its own line.
- For non-public methods, a comment under the `def` line is enough.

---

## Naming conventions

### Quick table (daily)

| What | Style | Example |
|------|-------|---------|
| Variable | `snake_case` | `user_name` |
| Function | `snake_case` | `get_total()` |
| Method | `snake_case` | `save_file()` |
| Class | `CapWords` | `UserProfile` |
| Exception | `CapWords` + `Error` | `ValidationError` |
| Constant | `UPPER_CASE` | `MAX_RETRIES` |
| Module | short `lowercase` | `utils.py` |
| Package | short `lowercase`, no underscores preferred | `mypackage` |
| Non-public | leading underscore | `_helper`, `_cache` |
| Keyword clash | trailing underscore | `class_` |
| Type variable | short `CapWords` | `T`, `AnyStr` |

Notes:
- Acronyms in class names stay in capitals: `HTTPServerError`, not `HttpServerError`.
- `mixedCase` (`getTotal`) is only for code that already uses it.
- First argument of an instance method is `self`. For a class method it's `cls`.
- Never use `l`, `O` or `I` as single-letter names. They look like `1` and `0`.
- Names with double underscores on both sides (`__init__`) are Python's own. Don't invent new ones.

### Leading underscores

```python
_single      # "internal use", a hint to other programmers.
             # `from m import *` skips these.
__double     # inside a class, gets name-mangled (to _ClassName__double)
```

Use `__double` only when a class is built to be subclassed and you need to avoid name clashes. Most of the time, `_single` is what you want.

### Public vs non-public

If unsure, make it non-public (`_name`). Making it public later is easy. Going the other way breaks users.

For simple data, expose a plain attribute. Don't write `get_x()` / `set_x()`. If you need logic later, switch to a `@property` without changing the callers.

Use `__all__` to list a module's public names.

---

## Programming recommendations

### None, booleans and truthiness (daily)

```python
# None: always is / is not
if foo is not None:        # good
if not foo is None:        # bad
if foo != None:            # bad

# Booleans: don't compare to True / False
if greeting:               # good
if greeting == True:       # bad
if greeting is True:       # worse

# Empty sequences are falsy
if not seq:                # good
if seq:                    # good
if len(seq):               # bad
```

Watch out: `if x:` is not the same as `if x is not None:`. A value like `0` or `[]` is falsy but not `None`. If you mean "was it set?", use `is not None`.

### Functions (daily)

```python
# Use def, not a lambda assigned to a name
def f(x):                  # good, shows up properly in tracebacks
    return 2 * x
f = lambda x: 2 * x        # bad
```

Be consistent with `return`. If any branch returns a value, all of them should, with an explicit `return None` where there's nothing to give back:

```python
# Good
def bar(x):
    if x < 0:
        return None
    return math.sqrt(x)

# Bad
def bar(x):
    if x < 0:
        return
    return math.sqrt(x)
```

### Exceptions (daily)

```python
# Catch specific exceptions
try:
    import platform_module
except ImportError:
    platform_module = None
```

- No bare `except:`. It also catches `Ctrl+C` and `SystemExit`. If you really need everything, write `except Exception:`.
- Your own exceptions inherit from `Exception`, not `BaseException`, and end with `Error`.
- Keep the `try` block as small as possible:

```python
# Good
try:
    value = collection[key]
except KeyError:
    return key_not_found(key)
else:
    return handle_value(value)

# Bad: also hides KeyErrors raised inside handle_value()
try:
    return handle_value(collection[key])
except KeyError:
    return key_not_found(key)
```

- Replace an exception and keep the original cause: `raise NewError(...) from err`.

### Resources (daily)

Use `with` so cleanup happens even on errors:

```python
with open("data.txt") as f:
    text = f.read()
```

### Strings and types

```python
if name.startswith("bar"):         # good
if name[:3] == "bar":              # bad

if isinstance(obj, int):           # good
if type(obj) is type(1):           # bad
```

In loops that build a long string, collect pieces and use `"".join(parts)` instead of repeated `+=`.

Don't write string literals that rely on invisible trailing spaces.

### Other

- Don't use `return`, `break` or `continue` inside a `finally:` block. It silently swallows exceptions.
- If you write one comparison method (like `__lt__`), consider writing all six, or use `functools.total_ordering`.

---

## Type annotations

Follow the same whitespace rules:

```python
def greet(name: str, times: int = 1) -> str:    # space around -> and around = when annotated
    ...

code: int
label: str = "unknown"      # no space before the colon, one after
```

Bad: `code:int`, `code : int`, `x: int=0`, `def f()->int:`.

Type checkers are optional tools. Python itself doesn't enforce annotations.

---

## Tools (extra)

You don't have to check all this by eye.

| Tool | Job |
|------|-----|
| `ruff` | very fast linter and formatter, replaces many tools |
| `black` | auto-formats code (default line length 88) |
| `flake8` / `pycodestyle` | report PEP 8 violations |

On Ubuntu, install them inside a venv:
```bash
pip install ruff
ruff check .          # find problems
ruff format .         # auto-format
```

Most teams run one of these in the editor and in CI, so style stops being something people argue about in code review. Tools like these often use 88 or 100 characters instead of 79, and that's fine, because PEP 8 itself allows teams to pick a longer limit.
