---
title: How to Set Up a Virtual Environment
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# How to Set Up a Virtual Environment

## Create the Environment

Use the `venv` package from Python's standard library. If you want the virtual
environment to be in VIRTUAL\_ENVIRONMENT\_DIRECTORY, then run:

```{console}
python3 -m venv --copies --clear VIRTUAL_ENVIRONMENT_DIRECTORY
```
This set of options deletes anything in VIRTUAL\_ENVIRONMENT\_DIRECTORY and
then makes copies of all of the system executables, placing them in `bin/`.

## `sudo` in the Virtual Environment

If you want to use `sudo` then you must specify the full path to the shell
command that you want to use. To run a Python module `serverthing` that hosts
on port 443, you could run the following command from the root of the virtual
environment:

```{console}
sudo bin/python3 -m serverthing 443
```
