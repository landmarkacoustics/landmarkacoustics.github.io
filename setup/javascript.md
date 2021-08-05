---
title: Structure a Javascript Project
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# How to Structure a Javascript Project

Just like in Python, the top-level directory of the project isn't the actual
code, but things like the license, readmes, configuration files, and some
subfolders.

## Webpack

I think I'm going to use [webpack](https://webpack.js.org) to organize my
javascript projects. That tool is based on [Node](https://nodejs.org). Webpack
has a great tutorial that I am working through.

### Initialize the project.

Make the directory and switch into it.

```console
mkdir PROJECT_NAME
cd PROJECT_NAME
```

Set up the files that Node uses to configure a package. This will create
``package.json``, which you will need to edit. For now, add a short description
at least, and anything else from [npm][1] that you can reasonably complete.

```console
npm init -y
```

Make sure that you exclude the `node-modules` directory from version control.
For example:

```console
echo "node-modules\n" > .gitignore
```

### Webpack

```console
npm install --save-dev webpack webpack-cli
```

## Source Folder

Webpack suggests calling this folder ``src``. The actual source files go here.
One file per class!

### Entry Point

One file should serve as the main entry point for the application. It should
have an obvious name like ``main.js``. You will load this file as a "module" in
your HTML. This file will load the dependencies and then start whataver event
listeners feature in your application.

### Subfolders

Each class should go in its own file. Related classes can be grouped together
into subfolders.

## Tests Folder

Make a folder for unit tests!

## Docs Folder

Make a folder called ``doc`` for any configuration or structure files that your
autodoc system may need.

## Build Folder

The results of bundling and/or compiling should go in ``build/dist``, and
auto-generated document generation should go in ``build/doc``.

[1]: https://docs.npmjs.com/cli/v6/configuring-npm/package-json
