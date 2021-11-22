---
title: How to Set Up Sphinx to Make Web Pages from Docstrings
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# Use Sphinx to Make Web Pages from Docstrings

Go to the package directory
## Update the setup configuration

Add the following block to ``setup.cfg``:

```ini
[build_sphinx]
project = replaceWithPackageName
version = replaceWithVersionNumber
release = replaceWithReleaseNumber
source-dir = sphinx
```

## Quickly build the Sphinx configuration

Make a new directory called ``sphinx``. This is where all of the ReStructured
Text files should go. They will define the layout of the webpages. Use the
following command to set up the configuration files and defaults:

```{console}
sphinx-quickstart sphinx \
	-p replaceWithPackageName \
	-a replaceWithAuthorName \
	-r replaceWithReleaseNumber \
	--ext-autodoc \
	--ext-doctest \
	--ext-intersphinx \
	--extensions=numpydoc,sphinx.ext.autosummary
```

The arguments have the following purposes:

``-p replaceWithPackageName``
: e.g. "lac_python_demo"

``-a replaceWithAuthorName``
: e.g. "Ben Taft"

``-r replaceWithReleaseNumber``
: e.g. "0.0.0", which Sphinx actually calls the "revision"

``--ext-autodoc``
: always use the autodoc extension!

``--ext-doctest``
: always use the doctest extension!

``--ext-intersphinx``
: for external documentation

``--extensions=numpydoc,sphinx.ext.autosummary``
: These extensions are bomb

## Edit the Python configuration file

Change the copyright value on line 22.

Add the following lines just below the "extensions" list:

```python
autosummary_generate = True
numpydoc_show_class_members = False
```

## Edit the ReST files so that Sphinx can find your code

This is how you tell Sphinx that the following code documents members of the
module ``replaceWithPackageName``, which is in the package ``PATH_TO``.

```rst
.. py:module:: PATH_TO.replaceWithPackageName
```
## Build the documents

Run the following command in the project's root directory, replacing
``$builder`` with either ``html``, ``xlm``, or ``latex``.

```console
python3 setup.py build_sphinx -b $builder
```

The documents will appear in ``./build/sphinx/${builder}``.

## Also check out this [tutorial][1]

[1]: https://sphinx-rtd-tutorial.readthedocs.io/en/latest/docstrings.html
