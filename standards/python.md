---
title: Coding Standards for Python
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# Standards for Python Code

[Numpy][1] is my primary inspiration for how to write docstrings. Of course, I
also try to follow the official Python code standards in [PEP 8][2]. The main
tool for enforcing standards is the linter [Pylint][3].

## File Layout

One class per file.
Related classes go into sub-packages.
the `__init__.py` imports classes with relative imports:
```{python}
from .module_file import ClassName
```


[1]: https://numpydoc.readthedocs.io/en/latest/format.html
[2]: https://www.python.org/dev/peps/pep-0008/
[3]: https://www.pylint.org

