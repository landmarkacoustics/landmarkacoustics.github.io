---
title: How to Set Up a Project that Uses Git for Version Control
author: Ben Taft
copyright: (C) 2020 Landmark Acoustics LLC
---

# How to Set Up a Project that Uses Git for Version Control

Create the folder that the project will be in
```console
mkdir projectname
```
Initialize the project
```{sh}
git init
```
Create your initial project structure

- [Python](./python/package.md)
- [Javascript](./javascript.md)

Stage those files as the first change to commit
```{sh}
git add -A .
```
Commit the changes
```{sh}
git commit -m "an appropriate comment"
```
Connect the local repository to github
```{sh}
git remote add origin https://github.com/username/projectname.git
git branch -M main
git push -u origin main
```
