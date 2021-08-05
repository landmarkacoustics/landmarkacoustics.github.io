---
title: Set up your own MySQL server
author: Ben Taft
copyright: 2020 by Landmark Acoustics LLC
---

# Set Up Your Own MySQL Server

These instructions pertain to Ubuntu. You don't have to, but the idea is to put
the MySQL server on a different machine than the web server.

## Log In to The Machine that will Serve MySQL

## Install MySQL Server on Your Machine
```console
sudo apt install mysql-server
```

## Configure the Server

### Purge insecure defaults
```console
sudo mysql_secure_installation
```

### Create a user

Start the MySQL shell
```console
sudo mysql
```

Create the root user and choose a password for it.
```mysql
ALTER USER 'root'@'localhost'
IDENTIFIED WITH mysql_native_password BY 'THEPASSWORDYOUCHOSE';
```

Quit MySQL
```mysql
QUIT;
```

### Tell MySQL to require secure remote access.
Halt the MySQL daemon.
```console
sudo service mysql stop
```

Have MySQL set up private-key encryption.
```console
sudo mysql_ssl_rsa_setup --uid=mysql
```

Open `/etc/mysql/my.cnf`.

#add the following line to the `[mysqld]` part:
```ini
    require_secure_transport=ON
```
#If this is a production server, change the listening ports.

Restart the MySQL demon
```console
sudo service mysql start
```

### Define the Data Model
Log in with the new, required, combination of password and key
```console
sudo mysql -u root -p --ssl-ca=/var/lib/mysql/ca.pem
```

Create the database that Django will use.
```mysql
CREATE DATABASE website
CHARACTER SET utf8
COLLATE uft8_general_ci;
```

Create a user for the Django application. In this example, it is called
'web' and it can access the MySQL server from any domain.
```mysql
CREATE USER 'web'@'%'
IDENTIFIED WITH mysql_native_password BY 'YOUR PASSWORD HERE'
REQUIRE X509;
```

Give that user the permissions it needs in order to function.
```mysql
GRANT ALL
ON website.*
TO 'web'@'%';
```

## Set Up the Webserver to Talk to Your MySQL Server
You will need to copy these files from `/var/lib/mysql` on the MySQL server
   to a temporary, but persistent, location on your Django machine.
   - `ca.pem`
   - `client-cert.pem`
   - `client-key.pem`

Log in to your Django Machine

After you have installed Django and run `startproject`, it should have a
   user account called `www-data`. Move the three `pem` files to the folder
   `/home/www-data/certs`.

Make `www-data` the owner of the files:
```console
sudo chown -R www-data:www-data /home/www-data
```

If everything went correctly then the 'web' user can log in to the mysql
   server using this command. Replace `MYSQL_SERVER_DNS_NAME_OR_IP_ADDRESS` and
   `MYSQL_SERVER_PORT` with the correct values, of course.
```console
 mysql \
	-h MYSQL_SERVER_DNS_NAME_OR_IP_ADDRESS:MYSQL_SERVER_PORT \
	-u web -p \
	--ssl-ca=ca.pem \
	--ssl-cert=client-cert.pem \
	--ssl-key=client-key.pem
```

