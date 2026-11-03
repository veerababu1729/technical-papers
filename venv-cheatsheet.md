# Python venv Cheat Sheet

---

## What is it?

A virtual environment is a private folder with its own Python and its own packages, made for one project.

Without it, every `pip install` goes to one shared place, so all your projects share the same packages.

- Project A needs `requests 2.25`
- Project B needs `requests 2.31`
- You can't have both installed globally, so one breaks

With a venv, each project gets its own box, and installs in one don't touch the others.

`venv` comes with Python. You don't need to install anything extra.

---

## Setup (Ubuntu)

One time only:
```bash
sudo apt install python3-venv python3-pip
```

---

## Step by step

**1. Go to your project folder**
```bash
mkdir myproject
cd myproject
```

**2. Create the environment**
```bash
python3 -m venv venv
```
The last `venv` is just the folder name. `.venv` is also common.

**3. Activate it**
```bash
source venv/bin/activate
```
Your prompt gets a prefix:
```
(venv) user@ubuntu:~/myproject$
```
That means you're inside.

Windows: `venv\Scripts\activate`

**4. Install packages**
```bash
pip install requests
pip install flask==3.0.0      # specific version
```
They go only into this project's venv.

**5. Run your code**
```bash
python app.py
```

**6. Deactivate when done**
```bash
deactivate
```

---

## Check you're inside

```bash
which python
```
It should point inside your project:
```
/home/you/myproject/venv/bin/python
```
If it shows `/usr/bin/python`, you're not activated.

---

## requirements.txt

A list of the packages your project needs, so anyone can rebuild the same setup.

```bash
pip freeze > requirements.txt          # save
pip install -r requirements.txt        # install from it
```

On a new machine:
```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

---

## Git

Don't commit the venv folder. Add this to `.gitignore`:
```
venv/
```
Commit `requirements.txt` instead.

---

## Delete an environment

```bash
deactivate
rm -rf venv     (r for recursive, f for force)
```
Nothing else is affected. You can make a new one any time.

---

## Command summary

| Task | Command |
|------|---------|
| Create | `python3 -m venv venv` |
| Activate | `source venv/bin/activate` |
| Deactivate | `deactivate` |
| Install | `pip install name` |
| List packages | `pip list` |
| Save packages | `pip freeze > requirements.txt` |
| Install from file | `pip install -r requirements.txt` |
| Delete | `rm -rf venv` |

---

## Common mistakes

- **Forgetting to activate.** You get `ModuleNotFoundError`, or packages land in the wrong place. Look for `(venv)` in your prompt.
- **`error: externally-managed-environment`.** Ubuntu 23.04+ blocks global `pip install` on purpose. Create and activate a venv, then install. Don't use `sudo pip` or `--break-system-packages`.
- **`ensurepip is not available`.** Run `sudo apt install python3-venv`.
- **Moving or renaming the venv folder.** It breaks. Make a new one and use `requirements.txt`.
- **Committing `venv/` to Git.** Add it to `.gitignore`.
- **A new terminal isn't activated.** Run `source venv/bin/activate` again in each new terminal.
