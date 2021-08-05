---
title: How to Set Up a Python Package
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# How to Set Up a Python Package

First, use [setuptools][1] (but see [PEP517][2]). Further detailed information
is available [here][3], but it seems outdated.

Update: use the `pip` package `build`.

## Files to Include in the Project Root

### README

Obviously!

### LICENSE

Try to make a good choice here

### pyproject.toml
A file that tells the setup system what to do when installing your package. 

```{ini}
[build-system]
requires = [
    "setuptools>=42",
    "wheel"
]
build-backend = "setuptools.build_meta"
```

### setup.cfg

```{ini}
[metadata]
name = replaceWithPackageName
version = replaceWithReleaseNumber
author = replaceWithAuthorName
author_email = replaceWithAuthorEmail
description = replaceWithShortDescription
long_description = file: README.md
long_description_content_type = text/markdown
url = replaceWithProjectURL
classifiers =
    Programming Language :: Python :: 3
    replaceWithLicenseType
    Operating System :: OS Independent

[options]
packages = find:

install_requires =
  requests
  importlib; python_version >= "3.0"

[options.packages.find]
where = .
include=patterns, to, match
exclude=patterns, to, match

```

### The replaceWithPackageName directory

This will have an `__init__.py` file in it, as well as any other modules, or
directories for sub-modules, that you need.

## Command to Build the Package

Make sure you are in the project root.

```{shell}
python3 -m build --no-isolation .
```
## Shortcut Script

There is a shortcut script in `~/Source/LAC/Scripts/create_python_project.sh`.

## Install the Package

Make sure you are in the project root

### Development Mode

The pep517 toolchain only creates compiled package documents, which are not
appropriate for development mode. To use that mode, you need a dummy `setup.py`
file that looks exactly like this:

```{python}
import setuptools
setuptools.setup()

```

```{shell}
sudo -H python3 -m pip install --editable .

```

### Global Mode

```{shell}
sudo -H python3 -m pip install .

```

[1]: https://setuptools.readthedocs.io
[2]: https://www.python.org/dev/peps/pep-0517/
[3]: https://packaging.python.org/tutorials/packaging-projects/
