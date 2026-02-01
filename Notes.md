```
sudo apt update
sudo apt install -y \
  build-essential \
  libssl-dev \
  zlib1g-dev \
  libbz2-dev \
  libreadline-dev \
  libsqlite3-dev \
  libffi-dev \
  liblzma-dev \
  xz-utils \
  tk-dev \
  libncursesw5-dev \
  libgdbm-dev \
  libnss3-dev \
  uuid-dev
```

```bash
curl https://pyenv.run | bash
```

Once pyenv is installed, install the Python version of your choice. You may have to install some dependencies to get this to work.

```bash
pyenv install 3.10.5
pyenv shell 3.10.5
exec $SHELL  # reload shell cache
python --version 
pyenv versions 
```
For systems that do not meet the Poetry requirements (Python versions >=3.9), use pyenv to upgrade your Python version before installing Poetry.



Now you are ready to install Poetry! First, make sure your system Python is selected. This makes sure Poetry remains available even if you decide to remove a specific version of Python.



Then, [install Poetry](https://python-poetry.org/docs/master/#installation):

```bash
curl -sSL https://install.python-poetry.org | python3 -
poetry --version
```

That's it! You should be ready to get started setting up your project. First, return to the system Python version: 
```bash
pyenv shell --unset
pyenv global system
pyenv versions
```

Before you do, I recommend configuring Poetry to create its virtual environments [in your project directory](https://python-poetry.org/docs/configuration/#virtualenvsin-project). This step is completely optional.

```bash
poetry config virtualenvs.in-project true
```

Now jump into practice. If you're the one creating the project:
```bash
cd ~/e-poetry
pyenv local 3.10.5  # or whatever version you want to use with that project 
poetry init 
```
or you can create the suggest structure from poetry with 
```bash
cd ~/e-poetry
# check to make sure you're using the latest version of Poetry
poetry self update
# change to the directory that will contain your new project
cd ~/repos
# create a new Poetry Python project
poetry new --src my_project --name my_package
```
Notes on your new Poetry project
```
Use the --src option to put your package code in a src subdirectory -- highly recommended!
Use the --name my_package option to give your package a different name than the project directory. Otherwise, skip this option to use the same name for your project and package directories.
```

Depending on the project and package names you selected, your new Poetry project directory should look something like this:
```tree 
my_project/
├── pyproject.toml
├── README.rst
├── src
│   └── my_package
│       └── __init__.py
└── tests
    ├── __init__.py
    └── test_my_package.py
```

- `pyproject.toml` - Project configuration and dependencies
- `README.md` - Project documentation
- `my_project/` - Your package source code
- `tests/` - Test files

Add these to pyproject.toml: 
```toml
[tool.poetry]
packages = [
    { include = "mypackage", from = "src" }
]
```
We have to tell Poetry to use the Python version we enabled above :
```bash
poetry env use $(pyenv which python)
```
This creates a virtual environment. You can find the location of the environment by typing poetry env info. It's a good idea to tell your IDE to use this environment; both VSCode and Pycharm have built-in support for Poetry environments.

```bash
poetry install  # create .venv to store dependencies
```

To add dependencies:
```bash
poetry add requests  # Add a package
poetry add numpy==1.26.4
poetry add --group dev pytest  # Add a dev dependency
```


Or run commands directly:
```bash
poetry run python -m mypackage.main  # or you can try poetry run python main.py
```

Results:
```bash
kyuubi@ubuntu:~/e-poetry$ poetry add numpy==1.26.4


Updating dependencies
Resolving dependencies... (0.5s)

Package operations: 1 install, 0 updates, 0 removals

  - Installing numpy (1.26.4)

Writing lock file
kyuubi@ubuntu:~/e-poetry$ 
kyuubi@ubuntu:~/e-poetry$ poetry run python -m mypackage.main

Array: [1 2 3]
Mean: 2.0
```


If you clone from others' Poetry projects:
```bash
git clone ...
cd project
poetry install
```

This will install all dependencies from the `pyproject.toml` and create a virtual environment.
