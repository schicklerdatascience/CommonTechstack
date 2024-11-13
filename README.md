# CommonTechstack
Documentation of a possible common techstack for Data Science projects. 


Agenda:
1. Briefly introduce the packages
2. Create a new poetry env for a new project (poetry init)
3. Install required python version with pyenv
4. Add some packages, remove, etc.
5. Setup pre-commit
6. The same once more with an existing project
7. SHow dockerfile to see 
8. Show pitfall

This repository covers the following tools/packages:

- __pyenv__ for Python Distribution management
- __poetry__ for virtual environment and packaging management
- __pre-commit__ for enforcing coding standards (formatting, linting, ...)

## Pyenv
- easily managing multiple Python versions on a single machine
- install and switch between different versions of Python

**For installation and general documentation see**:
https://github.com/pyenv/pyenv

**Useful commands**
- `pyenv versions` list all installed python versions
- `pyenv install <VERSION>` install Python version for example `pyenv install 3.10.6`
- `pyenv global <INSTALLED_PYTHON_VERSION>` sets the globally preferred python version
- `pyenv local <INSTALLED_PYTHON_VERSION>` sets the preferred local version 
    - Useful for individual projects to specify which python version should be used by pyenv for that project --> Creates .python_version file
- `pyenv uninstall <INSTALLED_PYTHON_VERSION>` uninstalls a python version

## Poetry
- dependency management and packaging tool for Python projects
- handles package installations, version conflicts, and dependency resolution automatically
- allows you to publish packages to PyPI easily
- creates an isolated virtual environment for each project
- creates two files:
    - pyproject.toml 
        - Projectkonfiguration, metadata
        - Specfies version constraints of required packages 
        - Can be adapted manually
        - **Delete the Readme.md metadata from the pyproject.toml!**
    - poetry.lock
        - Stores the exact versions of all installed dependencies
        - Shouldnt be changed by hand

**For installation and general documentation see**:
https://python-poetry.org/docs/

**Useful commands**
- `poetry init` only needed when creating a new project, guides you through the setup
    - Creates pyproject.toml file with main dependencies
- `poetry shell` activates or creates & activates the virtual env (poetry env)
    - equivalent to `virtualenv venv` + `source venv/bin/activate`
- `poetry add <PACKAGE>` adds packages to pyproject.toml file and installs it (equivalent to pip install) e.g. `poetry add pandas` 
- `poetry remove <PACKAGE>` uninstalls package from poetry environment
    - Possible: Version constraints, github dependencies, local packages, wheels
- `poetry install` reads the pyproject.toml file from the current project, resolves the dependencies, and installs them
    - After cloning or pulling latest changes from GitHub for example (dependencies might have changed)
- `poetry update <PACKAGE>` update versions for dependencies inside their version constraints specified in the pyproject.toml
- `poetry env list` lists poetry environments
- `poetry env remove <POETRY ENV>` deletes a poetry environment
- `poetry run <PATH>` runs a python file within the poetry env
    - not needed when poetry env is activated

**ATTENTION: There are two pitfalls**:
- Poetry expects a certain file structur, in fact the following:
    my-package  
    ├── pyproject.toml  
    ├── README.md  
    └── **my_package**  
        └── __init__.py  

- There are two options:
    1. Stick to that pattern (a folder) - Folder with the same name like the project
    2. Change the package_name in the pyproject.toml - `poetry init` asks for that
        - The poetry i.e. pyproject.toml package name need to satisfy PEP508 convention (https://peps.python.org/pep-0508/#names)
        my-package  
        ├── pyproject.toml  
        ├── README.md  
        └── **src**  
            └── __init__.py  
        - If your folder need to have a name that violates the PEP508 naming conventions you can add packages = [{include = "$34_weird_folder_name"}] to pyproject.toml [tool.peotry]
        my-package  
        ├── pyproject.toml  
        ├── README.md  
        └── **$34_weird_folder_name**  
            └── __init__.py  
- IN ANY CASE IT NEEDS A \__init\__.py file

## Pre-Commit
- framework for managing and running Git hooks (scripts that execute automatically at certain stages in the Git workflow)
- I use it to enforce code standards such as linting errors before every commit
    - Single .yaml file
    - IDE indepedendent code standards

**Usage**:
- Need to be installed once (system/user wide) with `pip install pre-commit`
- Need to be set up for the project once with `pre-commit install` or better add it to dependencies with `poetry add pre-commit`
- When creating a new project: create a file named .pre-commit-config.yaml 
- In an existing project the file should already exist (should be included in commits)
