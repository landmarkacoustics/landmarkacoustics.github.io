---
title: How to set up a Django Site
author: Ben Taft
copyright: 2020 Landmark Acoustics LLC
---

# HOW TO SET UP A DJANGO SITE
To run Django, you need a database so that user interactions have persistant
storage, a webserver that can, at minimum, handle HTTP requests, and Django
itself. You will probably want to put the database on its own machine. I think
that you have to keep the Webserver and Django process on the same machine.

## DATABASE
I have figured out three ways to use SQL as the database backend. They exhibit
an almost perfect correlation between hassle and usefulness. They are presented
here in ascending order of both.

### SQLite
This works right out of the box but isn't suitable for a production website.

### Dreamhost
Dreamhost supports virtual machines that host MySQL instances. Click
[here](./sql/dreamhost.md) for some thoughts on how to use them.

### Your Own MySQL Server
This is a lot more challenging. Click [here](./sql/own_server.md) if you
think that it's worth it to try.

## WEBSERVER
### APACHE
If you're using WSGI and a simple website-to-access-a-database approach, this
is the tried-and-true approach. The surpirisingly straightforward instructions
are [here](./webserver.md)

### DAPHNE
I'm going to try to incorporate some Websockets functionality into my websites,
so I think I need to use Channels and its server, Daphne.

## DJANGO
Maybe should install Django first, and then perform the other steps?

### Install Django

You need some things from Ubuntu.
```{console}
sudo apt install python3-pip libmysqlclient-core-5.7
```
You need other stuff from PyPl.
```{console}
sudo -H pip3 install django mysqlclient django-mysql-ssl
```

### Create a user for the site. Obviously, don't really use 'sitename'.

```{console}
sudo adduser sitename_user
su sitename_user
```

### Create the folder structure for the project.

Move to the sitename_user's folder and create a containing directory.
```{console}
cd ~
mkdir WWW
cd WWW
```
Start the project.
```{console}
django-admin startproject SITE_NAME
```
Add some custom folders that I like to use.
```{console}
cd SITE_NAME
mkdir html
mkdir mysql
```

### Initialize the Database that You Set Up Above

```{console}
sudo -u sitename_user python3 manage.py migrate
```

### Create an admin user for the Django site

```{console}
sudo -u sitename_user python3 manage.py createsuperuser
```
Don't forget to write down your username and password!!!!!

### Log in to the admin section of your site.
You should be able to go to `SITE_DNS_NAME_OR_IP:SITE_PORT/admin` now.
