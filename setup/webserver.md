---
title: Set Up an Apache WebServer for a Django Site
author: Ben Taft
copyright: 2020 by Landmark Acoustics LLC
---

# Set Up an Apache Webserver for a Django Site
## Make Sure pip is Installed.

```console
sudo apt install python3-pip
```

## Install Apache
```bash
sudo apt install apache-2-dev libapache2-modwsgi-py3
```

## Create a site configuration file for `mod_wsgi`
Copy one of the template files to create your site's file.

```{console}
cd /etc/apache2/sites-available
cp 000-default.conf sitename.conf
```

Edit `sitename.conf` so that `mod_wsgi` can learn what to do from Django.

You will need to change the following things that I'm putting in `ALL_CAPS`:

- `IP_ADDRESS` : default is `127.0.0.1`
- `PORT_NUMBER` : default is `8000`
- `SITE_NAME` : this was in Camel Case in my original document.
- `SITE_APPLICATION` : this is all in lower case in my original document.

### The edited file should have several changes
```{apache}
Listen IP_ADDRESS:PORT_NUMBER

<VirtualHost *:PORT_NUMBER>
  Define my_site_name SITE_NAME
  Define my_site_root "/home/site_user/WWW/${my_site_name}"
  ...
  ServerAdmin webmaster@localhost
  DocumentRoot "${my_site_root}/html"
  ...
  Alias /static/ "${my_site_root}/static/"
  <Directory "${my_site_root}/static" >
    Require all granted
  </Directory>
  
  WSGIScriptAlias / "${my_site_root}/${my_site_name}/wsgi.py"
  WSGIDaemonProcess SITE_APPLICATION python-path="${my_site_root}"
  WSGIProcessGroup SITE_APPLICATION

  <Directory "${my_site_root}/${my_site_name}">
    <Files wsgi.py>
      Require all granted
    </Files>
  </Directory>  
</VirtualHost>
```
