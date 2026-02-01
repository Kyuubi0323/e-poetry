# Poetry Project Setup Guide

This guide will help you set up a Python project using Poetry for dependency management. Whether you're creating a new project or contributing to an existing one, follow these steps to get started.


## Table of Contents
- [Prerequisites](#prerequisites)
- [Installing Poetry](#installing-poetry)
- [Configuring Poetry](#configuring-poetry)
- [Creating a New Project](#creating-a-new-project)
- [Setting Up Your Environment](#setting-up-your-environment)
- [Managing Dependencies](#managing-dependencies)
- [Running Your Project](#running-your-project)
- [Working with Existing Poetry Projects](#working-with-existing-poetry-projects)


## Prerequisites

### 1. Install System Dependencies

First, install the required system dependencies for building Python:

```bash
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

### 2. Install pyenv

```bash
curl https://pyenv.run | bash
```

> [!WARNING]
> Disable others virtual environments first like (conda deactivate)
>
> Then export these only in this shell
>
> ```bash
> export PYENV_ROOT="$HOME/.pyenv"
> export PATH="$PYENV_ROOT/bin:$PATH"
> eval "$(pyenv init -)"
> ```

Once pyenv is installed, install the Python version of your choice. You may have to install some dependencies to get this to work.

```bash
pyenv install 3.10.5
pyenv shell 3.10.5
exec $SHELL  # reload shell cache 
python --version 
pyenv versions 
```
**Note:** For systems that do not meet Poetry's requirements (Python versions >=3.9), use pyenv to upgrade your Python version before installing Poetry.

## Installing Poetry

Now you are ready to install Poetry! First, make sure your system Python is selected. This ensures Poetry remains available even if you decide to remove a specific version of Python.

```bash
pyenv shell --unset
pyenv global system
pyenv versions
```

Then, [install Poetry](https://python-poetry.org/docs/master/#installation):

```bash
curl -sSL https://install.python-poetry.org | python3 -
poetry --version
```

## Configuring Poetry

I recommend configuring Poetry to create its virtual environments [in your project directory](https://python-poetry.org/docs/configuration/#virtualenvsin-project). This step is completely optional but makes project management easier:

```bash
poetry config virtualenvs.in-project true
```

## Creating a New Project

There are two ways to create a new Poetry project:

### Option 1: Initialize in Existing Directory

If you already have a project directory:

```bash
cd ~/my_project
pyenv local 3.10.5  # or whatever version you want to use with this project 
poetry init
```

### Option 2: Create New Project with Recommended Structure

Use Poetry to create a new project with the recommended structure:

```bash
# Check to make sure you're using the latest version of Poetry
poetry self update

# Change to the directory that will contain your new project
cd ~/repos

# Create a new Poetry Python project
poetry new --src my_project --name my_package
```

**Important Options:**
- Use the `--src` option to put your package code in a `src` subdirectory (highly recommended!)
- Use the `--name my_package` option to give your package a different name than the project directory. Otherwise, skip this option to use the same name for both project and package directories.

### Project Structure

Depending on the project and package names you selected, your new Poetry project directory should look like this:

```
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

**Key Files and Directories:**
- `pyproject.toml` - Project configuration and dependencies
- `README.rst` - Project documentation
- `src/my_package/` - Your package source code
- `tests/` - Test files

## Setting Up Your Environment

### Configure Package Location (if using src layout)

If you're using the `src` directory layout, add this to your `pyproject.toml`: 
```toml
[tool.poetry]
packages = [
    { include = "mypackage", from = "src" }
]
```

### Set Python Version

Tell Poetry to use the Python version you want for this project:

```bash
poetry env use $(pyenv which python)
```

This creates a virtual environment. You can find the location of the environment by typing `poetry env info`. It's a good idea to tell your IDE to use this environment; both VS Code and PyCharm have built-in support for Poetry environments.

### Install Dependencies

Install all project dependencies:

```bash
poetry install  # Creates .venv to store dependencies
```

## Managing Dependencies

### Adding Dependencies

To add new dependencies to your project:
```bash
poetry add requests  # Add a package
poetry add numpy==1.26.4  # Add a specific version
poetry add --group dev pytest  # Add a dev dependency
```

## Running Your Project

### Activate Virtual Environment

You can activate the Poetry virtual environment:

```bash
poetry shell
```

### Run Commands Directly

Or run commands directly without activating the environment:
```bash
poetry run python -m mypackage.main  # Run as a module
# or
poetry run python main.py  # Run script directly
```

### Example Output
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

## Working with Existing Poetry Projects

If you're cloning someone else's Poetry project:

```bash
git clone <repository-url>
cd project
poetry config virtualenvs.in-project true
poetry install
```

This will:
1. Read the `pyproject.toml` and `poetry.lock` files
2. Create a virtual environment
3. Install all dependencies specified in the lock file

## Useful Poetry Commands

```bash
poetry --version          # Check Poetry version
poetry env info           # Show environment information
poetry env list           # List all virtual environments
poetry show               # List installed packages
poetry show --tree        # Show dependency tree
poetry update             # Update dependencies
poetry add <package>      # Add a new dependency
poetry remove <package>   # Remove a dependency
poetry lock               # Update poetry.lock without installing
poetry export -f requirements.txt --output requirements.txt  # Export to requirements.txt
```

## Best Practices

1. **Always commit `poetry.lock`**: This ensures everyone uses the same dependency versions
2. **Use `--group dev` for development dependencies**: Keeps production dependencies separate
3. **Configure `.gitignore`**: Add `.venv/` or `*venv/` to ignore virtual environments
4. **Use `poetry shell`**: Activates the environment for your current terminal session
5. **Keep Poetry updated**: Run `poetry self update` regularly

## Additional Resources

- [Poetry Documentation](https://python-poetry.org/docs/)
- [pyenv Documentation](https://github.com/pyenv/pyenv)
- [Python Packaging Guide](https://packaging.python.org/)
