---
title: How to Set Up a Django Application
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# How to Set Up a Django Application

The project will be in `PROJECT_NAME` and the application will be `APP_NAME`.
## 1. Create a Project Directory

This doesn't have to be a valid package name.
It is the location that will hold all of your files, including documentation
and tests.

```{console}
mkdir PROJECT_NAME
```

### Add the Stuff Python Needs for Packaging

#### setup.cfg

Some nice default suggestions come from [python][1] and [django][2]

```{ini}
[metadata]
name = APP_NAME
version = 0.0.0
description = SHORT_APP_DESCRIPTION
long_description = file: README.md
url = https://github.com/landmarkacoustics
author = Ben Taft
author_email = ben.taft@landmarkacoustics.com
license = MIT
classifiers =
    Environment :: Web Environment
    Framework :: Django
    Framework :: Django :: 3.1
    Intended Audience :: Developers
    License :: OSI Approved :: BSD License
    Operating System :: OS Independent
    Programming Language :: Python
    Programming Language :: Python :: 3
    Programming Language :: Python :: 3 :: Only
    Programming Language :: Python :: 3.6
    Programming Language :: Python :: 3.7
    Programming Language :: Python :: 3.8
    Topic :: Internet :: WWW/HTTP
    Topic :: Internet :: WWW/HTTP :: Dynamic Content

[options]
include_package_data = true
packages = find:

install_requires =
  requests
  importlib; python_version >= "3.0"

[options.packages.find]
where = .
include=
exclude=

[build_sphinx]
project = APP_NAME
version = 0.0
release = 0.0.0
source-dir = docs
```

#### MANIFEST.in

```{ini}
include LICENSE
include README.md
recursive-include APP_NAME/static *
recursive-include APP_NAME/templates *
recursive-include build/sphinx/html *
```

#### README.md

```{markdown}
# APP_NAME

LONG_APP_DESCRIPTION
```

### LICENSE

I usually prefer the [MIT license][3]

### These last two files are always the same.

#### setup.py

```{python}
import setuptools
setuptools.setup()
```

#### pyproject.toml

```{ini}
[build-system]
requires = ["setuptools", "wheel"]
build-backend = "setuptools.build_meta"
```

## 2. Create the Django Application Skeleton

`django-admin` gives a good starting point.

```{console}
cd PROJECT_NAME
django-admin startapp APP_NAME
```

### Change the Modules to Subpackages

```{console}
for r in models views; do
    mkdir $r
	mv $r.py $r/default.py
	touch $r/__init__.py
	done
```

### Update the Package's `__init__.py` File

Note that this MUST NOT import `models` or `views`!
It turns out that this is super critical.
Django will import each apps' model and view modules after loading all of the
`INSTALLED_APPS`.

```{python}
# Copyright (C) YEAR by Landmark Acoustics LLC
r"""A brief docstring for Sphinx."""

# this space intentionally left blank

```

### Create a Stub `urls.py` File

put this in `APP_NAME/urls.py`:

```{python}
# Copyright (C) YEAR by Landmark Acoustics LLC

from django.urls import (
    include,
    path,
)

urlpatterns = [
]
```

## 3. Set Up the Sphinx Documentation System

Create a documentation directory and run sphinx-quickstart.

```{console}
cd ..
mkdir docs
sphinx-quickstart docs
```

### Create a Settings File

Put this in `docs/doc_settings.py`.

It must contain a secret key.
***THIS MUST NOT BE ANY KIND OF PRODUCTION SECRET KEY!!!!***

You should also list all of the apps that your app depends upon, and then your
app itself.

```{python}
SECRET_KEY = 'not so secret'

INSTALLED_APPS = [
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'APP_NAME',
]
```

### Edit the Documentation's `conf.py` File

- Change the 'Path setup' section to the following

```{python}
import os
import sys
sys.path.insert(0, os.path.abspath('..'))

import django
os.environ['DJANGO_SETTINGS_MODULE'] = 'docs.doc_settings'
django.setup()
```

### Add Extensions and Configure Them

Make the following extensions as string elements of the `extensions` list:

```{python}
extensions = [
    'sphinx.ext.autodoc',
    'sphinx.ext.doctest',
    'sphinx.ext.intersphinx',
    'numpydoc',
    'sphinx.ext.autosummary',
	'sphinxcontrib_django',
]
```

Add the following configuration options after the extension list:

```{python}
autosummary_generate = True
numpydoc_show_class_members = False

intersphinx_mapping = {
    'python': (
	    'https://docs.python.org/3',
	    None
	),
    'django': (
        'http://docs.djangoproject.com/en/stable/',
        'http://docs.djangoproject.com/en/stable/_objects/'
	),
}
```

### Populate the REStructured Text Files

The three-level hierarchy that I describe here seems obtuse, but it yields very
nice documentation pages that are easy to traverse.

#### The Root of the Documentation

Describe the landing page for the documentation with `docs/index.rst`.
Its contents should include a title for the documentation site, an `automodule`
directive that points to the app, and a `toctree` directive that includes, at
least, the `models` and `views` modules. It should look something like this:

```{rest}
======================
APP_NAME: a Django app
======================

.. automodule:: APP_NAME

.. toctree::
   :maxdepth: 2

   models
   views

==================
Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`
```

Notice that the headings on this page are surrounded by equals signs.

#### Documentation for Submodules

Each submodule should be documented by main file with its name and a `toctree`
that points to files for each of its classes. The headings in a file that
documents a submodule should have equals signs underneath them. For example,
the `models` module should have a `models.rst` file that looks like this:

```{rest}
Models
======

.. automodule:: APP_NAME.models

.. toctree::
   :maxdepth: 2

   models/model_class
```

#### Documentation for Classes

The files that describe the submodule's classes should all go into a directory
that has the same name as the submodule. The files themselves should have the
same basename as the file where the class is defined. The example above shows a
submodule called `models` with information in `models_class`.

The class module's file should have the class name as a heading with dashes
underneath it and an `autoclass` directive. It should look something like this:

```{rest}
ModelClass
----------

.. autoclass:: APP_NAME.models.ModelClass
```

### Build the Documents into a Web Page

```{console}
python3 setup.py build_sphinx -b html
```


## 4. Set Up the Testing Environment

I use [pytest][4] as my unit testing framework for my non-Django python work.
There are some 3rd-party packages to integrate pytest and Django, such as
[pytest-django][5]. It seems pretty mature, but is aimed at testing projets
rather than applications, so it isn't working really well for me.

### Create the Test Folders

The tests folder should go in the project's root directory, next to `docs` and
the `APP_NAME` folders. It will be a python module, so include a blank
`__init__.py` file. You'll also want test folders for models, views, and
templates.

```{console}
cd PATH/TO/PROJECT_NAME
mkdir tests
touch tests/__init__.py
for r in models views test; do
    mkdir $r;
	touch $r/__init__.py;
	done;
```

### Make the Django Settings for the Test Runs

You should call this file something like `settings_for_tests.py`, rather than
`test_settings.py` because the latter name will be found by test discovery! The
file should include these things:

```{python}
SECRET_KEY = 'not so secret'

DEBUG = True

ALLOWED_HOSTS = []

INSTALLED_APPS = [
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'APP_NAME',
]

MIDDLEWARE = [
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
]

DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': 'tests/sqlite3.db'
    }
}

ROOT_URLCONF = 'APP_NAME.urls'

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            'templates'
        ],
        'APP_DIRS': True,
     },
]

TIME_ZONE = 'America/Chicago'
USE_TZ = True

AUTHENTICATION_BACKENDS = [
    'django.contrib.auth.backends.ModelBackend'
]
```

### Create the necessary migrations

Run this command from the project's root directory:
```{console}
django-admin migrate --pythonpath='.' --settings="tests.settings_for_tests"
```

### Write Set Up Code for the Test Environment

The `tests/conftest.py` file is run before all other tests, so it is where you
can put all of your setup code. Include the information that you need to run
Django:

```{python}
# Copyright (C) 2021 by Landmark Acoustics LLC

import os
import sys

import pytest
import django

from django.apps import apps
from django.contrib.auth import get_user_model


sys.path.insert(0, os.path.abspath('..'))
os.environ['DJANGO_SETTINGS_MODULE'] = 'tests.settings_for_tests'
django.setup()


@pytest.fixture(scope='session')
def DjangoUserModel():
    r"""The class that corresponds to user objects in the auth database."""
    return get_user_model()


@pytest.fixture(scope='session',
                autouse=True)
def django_database(DjangoUserModel):
    r"""Set the database up and then tear it down.

    This runs after test collection but before the other tests. It will run
    just once with the above arguments to `@pytest.fixture`. Put the setup code
    before the `yield` statement and the teardown code after the `yield`.

    """

    # uncomment to clear database
    # for app_name in ['APP_NAME']:
    #     app_config = apps.get_app_config(app_name)
    #     for model in app_config.get_models():
    #         print(f'clearing {model}')
    #         for obj in model.objects.all():
    #             print(f'deleting {obj}')
    #             obj.delete()

    # for user in DjangoUserModel.objects.all():
    #     print(f'deleting user "{user}"')
    #     user.delete()

    yield None


@pytest.fixture(scope='session')
def create_django_user(DjangoUserModel):
    r"""A User factory."""

    created_users = set()

    def _user(**kwargs):
        user = DjangoUserModel.objects.create_user(**kwargs)

        created_users.add(user)

        return user

    yield _user

    while created_users:
        user = created_users.pop()
        user.delete()
```

## 5. Add Some Classes

### Model code

- Create the class file in `APP_NAME/{models|views}`.
- Make sure the class has some docstrings.
- Update `{models|views}/__init__.py` to import the class

```{python}
from .class_file import {Model|View}Class
```

### Database migrations
Once you have added the classes, you need to update the database.
Run the following command from the root directory of the project:

```{console}
django-admin makemigrations \
	APP_NAME \
	--pythonpath='.' \
	--settings="tests.settings_for_tests"
```

You'll also have to update your test database.

```{console}
django-admin migrate --pythonpath='.' --settings="tests.settings_for_tests"
```

[1]: https://packaging.python.org/tutorials/packaging-projects/
[2]: https://docs.djangoproject.com/en/3.1/intro/reusable-apps/
[3]: https://choosealicense.com/licenses/mit/
[4]: https://docs.pytest.org/en/stable/
[5]: https://pytest-django.readthedocs.io/en/latest/index.html
