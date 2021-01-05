# Setting up VSCode for Developing Machine Learning Applications in Python

## What you get from setting up the IDE this way
- The IDE will help you catch errors early
- The IDE will help you (and your team mates) write consistent code
- The IDE will make you more productive

## Setting up the Python interpreter

Use [`pyenv`](https://github.com/pyenv/pyenv) to manage multiple Python installations on your computer. You should never install packages into the your system's default Python installation. Instead, you can use `pyenv` to activate and deactivate different, isolated versions of Python.

**Helpful commands:**

- `pyenv install -v 3.8.5`  to install Python 3.8.5
- `pyenv global 3.8.5` to activate that version globally
- `python --version` to check the current Python version

**Helpful links:**

- [Project documentation](https://github.com/pyenv/pyenv)
- [Realpython article](https://realpython.com/intro-to-pyenv/)

## Setting up virtual environments

Each project should have its own virtual environment. The basic functionality is provided by Python's native [`venv`](https://docs.python.org/3/tutorial/venv.html). The workflow for setting up a new project consists of:

- Setting up a `git` repo
- Installing and activating the right python version, e.g. through `pyenv global 3.8.5`
- Creating a virtual environment through `python -m venv /path/to/virtualenv`
- Optional: Install requirements from  `requirements.txt`

You can either store your virtual environments in a central place, e.g. `~/.virtualenvs/project-a-venv` or within your project directory under `./venv`. In the latter scenario, make sure that folder is not part of version control. 

There are multiple popular helpers for managing virtual environments, e.g. [`virtualenvwrapper`](https://virtualenvwrapper.readthedocs.io). Those tools are mainly helpful for browsing and quickly switching between virtual environments.

## Using the Command Palette

On Mac, you can use `CMD + Shift + P` to open the command palette. This is a good place to start whenever you don't know how to do something.

Note: In this guide, I am going to use italics to indicate queries for the command palette, e.g. *>Python: Select Interpreter* refers to hitting `CMD + Shift + P` and then entering "Python: Select Interpreter".

## Installing the official Python extension

The Python extension provides language-specific functionality for Python (syntax highlighting, linting, formatting, etc.). VSCode will normally prompt you to install it when opening a Python file, but you can also [install it manually](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

## Installing Pylance and selecting the right interpreter

Pylance provides extended functionality for Python. You can install it through the [marketplace](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-pylance).

You also need to tell VSCode which Python interpreter to use for the project at hand (*>Python: Select Interpreter*). The interpreter is part of the virtual environment that you have created before. The IDE will then use that environment for analysing your code, running the debugger, and for the interactive window.

## Environmental Variables

Secrets and local configuration are typically handled in environmental variables. Never add secrets to version control. You can put a file called `.env` into your project's root directory and tell VSCode to set those variables everywhere by setting `python.envFile` to `${workspaceFolder}/.env` in your user or project settings. This repo contains sample user settings (see section on the *Settings File* below).

## Interactive Window

You can start an interactive window (based on Jupyter) by selecting code and hitting `CMD + Enter`. This is very useful for exploratory work. Note that interactive mode requires the package `ipykernel` to be installed (VSCode will prompt you to install it if you don't have it).

By default, Jupyter is launched from the current file's root folder. This can be annoying because relative imports will behave unexpectedly. To avoid this problem, you can set the `jupyter.notebookFileRoot` to `${workspaceFolder}`.

## Debugging

The VSCode debugger is extremely powerful. Just set a breakpoint within any function and then hit "Run and Debug" in the debugging tab. 

## Testing (pytest)

For unit testing, place your tests into a folder `tests` and enable configure unit testing through *>Python: Configure Tests.* Your tests will then show up in a new tab on the left.

## Formatting (black)

You can format any file with *>Format Document.* By default, VSCode will use `black` for formatting and prompt you to install it if you haven't done so already. You can also format files upon saving automatically by setting `editor.formatOnSave` to `true`. 

## Linting (flake8)

The Python extension will automatically lint your code and tell you about any potential problems. To do that, you need to have `flake8` installed. VSCode will normally prompt you to install it.

## Sorting Imports (isort)

You can automatically sort your imports with `isort` by through *Python Refactor: Sort Imports*. For compatibility with `black`, you need to set `python.sortImports.args` to `["--profile=black"]`.

## Installing a local package

Most projects will make use of Python's packaging system, i.e. contain a `setup.py` or [`pyproject.toml`](https://setuptools.readthedocs.io/en/latest/userguide/quickstart.html) at the root level. For a project in "src layout", you will usually install the package in editable mode through  `pip install --editable .`. Then, you can import from the package in any other python script or notebook (e.g. `from src.utils import foo`) and you won't need to re-install after editing the package (that's why it's called "editable" mode).

## Settings File

This repo contains a pre-written user settings file ([`settings.json`](settings.json) that you can place into `~/Library/Application Support/Code/User/settings.json`. All projects will inherit the user settings. Projects can extend/overwrite user settings by providing another file in the project directory under `.vscode/settings.json`. Pylance, for example, will automatically add an entry related to the location of the Python interpreter in the project settings.

## Checking if it works
You can verify the following behavior to make sure everything works:
- The IDE should notice unused imports.
- The IDE should automatically replace `x='abc'` with `x = "abc"` upon saving a file.
- The IDE should automatically sorts imports, e.g. `import pandas as pd` is moved below `import os` upon saving.
- Interactive mode starts with from root directory even if the file is in a subdirectory, e.g. you can interactively run a script `scripts/train.py` and still write `pd.read_csv("data/train.csv")`.
- `flake8` and `black` are compatible, i.e. a line that is 87 characters long is not flagged by `flake8` whereas a line that is 89 characters long is. `black` will try to break lines that are longer than 88 characters.
- Unit tests with `pytest` work out of the box.

# Additional Topics and Extensions

- Collaboration with the [Live Share Extensions](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare-pack)
- [Remote development](https://code.visualstudio.com/docs/remote/ssh) through SSH (this is great for working on beefy GPU virtual machines in ML projects)
- [Developing inside a Docker container](https://code.visualstudio.com/docs/remote/containers)
- Shortcuts for [Mac](https://code.visualstudio.com/shortcuts/keyboard-shortcuts-macos.pdf)
- [Vim extension](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim)
- Automatic [docstring generation](https://marketplace.visualstudio.com/items?itemName=njpwerner.autodocstring)
