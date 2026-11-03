# Python pip Cheat Sheet

---

## What is pip?

pip is Python's package installer. It downloads packages (libraries other people wrote) from PyPI (pypi.org), the official package store, and installs them.

Want to make web requests? Don't write it yourself:
```bash
pip install requests
```

```python
import requests
print(requests.get("https://example.com").status_code)
```

pip comes with Python. On Ubuntu you may need to install it once.

---

## Setup (Ubuntu)

```bash
sudo apt install python3-pip
pip --version
```

You'll see something like:
```
pip 24.0 from /usr/lib/python3/dist-packages/pip (python 3.12)
```

**Always use pip inside a venv.** Ubuntu 23.04+ blocks global installs:
```
error: externally-managed-environment
```

So the routine is:
```bash
python3 -m venv venv
source venv/bin/activate
pip install requests
```

Inside an activated venv, plain `pip` is the right one.

---

## Installing

```bash
pip install requests                # latest version
pip install requests==2.31.0        # exact version
pip install "requests>=2.25"        # at least this version
pip install "requests<3"            # below this version
pip install requests flask          # several at once
```

| Symbol | Meaning |
|--------|---------|
| `==` | exactly this version |
| `>=` | this version or newer |
| `<=` | this version or older |
| `<` | older than this |
| `~=` | compatible release (`~=2.31` means 2.31 or newer, below 3.0) |

Put quotes around version ranges. Without them, `>` and `<` are treated as shell redirects.

---

## Upgrading

```bash
pip install --upgrade requests      # or -U
pip install -U pip                  # upgrade pip itself
```

---

## Uninstalling

```bash
pip uninstall requests
pip uninstall -y requests           # skip the "are you sure" question
```

This removes only that package. The extra packages it pulled in stay installed.

---

## Looking around

```bash
pip list                    # everything installed
pip show requests           # details: version, location, what it needs
pip list --outdated         # which ones have newer versions
pip check                   # finds broken or conflicting dependencies
```

Finding packages: search on pypi.org. The old `pip search` command no longer works.

---

## requirements.txt

A list of what your project needs, so anyone can rebuild the same setup.

```bash
pip freeze > requirements.txt       # save what's installed now
pip install -r requirements.txt     # install everything from the file
```

The file looks like this:
```
certifi==2024.2.2
requests==2.31.0
urllib3==2.2.1
```

`pip freeze` lists every package, including the ones installed automatically as dependencies. That's fine, because it locks the exact versions.

Commit this file to Git. Don't commit the `venv` folder.

---

## Dependencies

Some packages need other packages. pip installs those for you.

```bash
pip install requests
```
This also installs `urllib3`, `certifi`, `idna` and `charset-normalizer`. You can see this with `pip show requests` (look at the `Requires` line).

---

## python -m pip

```bash
python3 -m pip install requests
```

This runs the pip that belongs to that exact Python. It's the safest form when:
- `pip` isn't found
- you have more than one Python installed
- packages seem to install but `import` fails

---

## Install from other places (less common)

```bash
pip install ./my_package                              # a local folder
pip install git+https://github.com/user/repo.git      # a Git repo
```

---

## Command summary

| Task | Command |
|------|---------|
| Check version | `pip --version` |
| Install | `pip install name` |
| Install exact version | `pip install name==1.2.3` |
| Upgrade | `pip install -U name` |
| Uninstall | `pip uninstall name` |
| List installed | `pip list` |
| Show details | `pip show name` |
| Show outdated | `pip list --outdated` |
| Save list | `pip freeze > requirements.txt` |
| Install from list | `pip install -r requirements.txt` |
| Use the right Python's pip | `python3 -m pip ...` |

---

## Common mistakes

- **`error: externally-managed-environment`.** You're outside a venv on a newer Ubuntu. Activate a venv first. Don't use `sudo pip` or `--break-system-packages`.
- **`pip: command not found`.** Run `sudo apt install python3-pip`, or activate your venv.
- **Installed it but `ModuleNotFoundError`.** pip and your code are using different Pythons. Activate your venv, or use `python3 -m pip install name`.
- **Wrong package name.** The install name can differ from the import name. `pip install opencv-python` is used as `import cv2`. `pip install pillow` is used as `import PIL`.
- **`sudo pip install`.** Don't. It can break system tools that Ubuntu relies on.
- **Forgetting quotes on version ranges.** `pip install requests>=2` fails. Write `pip install "requests>=2"`.
- **No `requirements.txt`.** Run `pip freeze > requirements.txt` before sharing your project.
- **Installing random packages without checking.** Look at the name on pypi.org first. Typo-named fake packages exist.
