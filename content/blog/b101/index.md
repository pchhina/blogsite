---
title: "Python Project Organization"
date: 2024-11-30T10:00:00-06:00
draft: false
tags: ["Python", "Software Design"]
---

Understanding how to organize Python projects using modules, packages, and namespaces is essential for building maintainable code. This guide presents a mental model for thinking about project structure.

## Core Abstractions

Think of a project in terms of four abstractions:

- **Objects** - Data and function definitions
- **Modules** - Python files containing one or more objects
- **Packages** - Directories containing one or more modules
- **Namespaces** - Names of modules and packages accessible in your code

A module is any Python file containing data and function objects. The top-level is the directory where your main module lives. Each module's objects should be tested independently before importing into other modules. At the top-level, use absolute imports to create namespaces from sibling modules.

## Namespaces

Namespaces allow you to use another module's tree structure of names. Only the top-level imported names are directly available to your module. Think of it as a tree where leaves are data and function objects, and branches are modules and packages. The dot operator traverses this tree.

Namespaces are created using `import` statements:

```python
import sys                      # sys tree is available
import numpy as np              # np tree is available  
from math import sqrt           # only sqrt object is available
import matplotlib.pyplot as plt # only pyplot sub-tree is available
```

When designing an abstraction, pay attention to which namespaces you create and rely on. When using an abstraction, you don't need to worry about its internal namespaces.

## Creating Packages

Add a blank `__init__.py` file to any directory that will contain Python modules. This makes it a valid Python package.

A package can contain:
- Subdirectories that are not packages (for data, configs, etc.) - access these with `pathlib`
- Subdirectories that are packages (sub-packages) - these become part of the main package and support relative imports

## Absolute and Relative Imports

### Absolute Imports

Start from the top-level directory:

```python
import p.a.b as b
```

### Relative Imports

Start from the calling module's location:

```python
from .b import B    # import object B from sibling module b
from . import b     # import entire sibling module b
from ..c import C   # c module exists one level up
```

Relative imports are only valid within a package. A module run directly as the main script cannot use relative imports without the `-m` flag.

## Importing Packages

When you `import p` where p is a package, only `__init__.py` runs. The entire tree is not automatically available. You have two options:

**Expose modules in `__init__.py`:**
```python
from . import pm
```
Then in your module:
```python
import p
p.pm.f_in_pm()
```

**Or import the module directly:**
```python
from p import pm
pm.f_in_pm()
```

## Running Modules

From IPython, changes to the top-level module are registered within the same session. Changes in called modules require restarting IPython as they're not reflected in `sys.modules`.

To run a module inside a package from the top-level:

```bash
ipython> run 'p/m.py'    # runs as if from inside p directory
```

**Recommended approach from terminal:**
```bash
$ python -m p.m          # runs from where p exists as package
```

This allows absolute imports to start from the correct top-level or use relative imports within the same package.

## Exercises

Run these exercises to strengthen your mental model:

**Basic imports:**
- Create two modules at top-level
- Import entire module and use its objects
- Import only one object and use it

**Package with one module:**
- Create a package at top-level with one module inside
- Import the entire module and use its objects
- Import one object from the module
- Expose the module at package level and import the package

**Sibling module imports:**
- Create another module in the package
- Import (absolute) a sibling module and use its object
- Import (absolute) an object from a sibling module
- Import (relative) a sibling module and use its object
- Import (relative) an object from a sibling module
- Run each case from terminal

**Cross-package imports:**
- Create another package at top-level with one module
- Import a module from sibling package and use its object

**Working with data files:**
- Create a data directory at top-level with a markdown file
- Use this file in one of the package's modules
