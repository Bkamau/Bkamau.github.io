---
layout: post
title: Nextcloud on Ubuntu 16.04
subtitle: Easy quick setup
---

Nextcloud is the new service that wants to replace owncloud. To make a long story short, this is how you install nextcloud on ubuntu 16.04

First update system. Become root if u have to.

```bash
 $ apt update && apt -y upgrade
```

Install lamp server and needed modules.

```bash
 $ apt install -y lamp-server^ && apt install -y libxml2-dev php-zip php-dom php-xmlwriter php-xmlreader php-gd php-curl php-mbstring && sudo a2enmod rewrite
```

Grab the latest release from <a href="https://nextcloud.com/install/#instructions-server">here</a>, unpack it and move it to web root folder

```bash
 $ cd /tmp && wget https://download.nextcloud.com/server/releases/nextcloud-9.0.51.tar.bz2
 $ tar -vxjf nextcloud-9.0.51.tar.bz2 && mv nextcloud /var/www/
```

Create a file that will set the neccesary permisions for nextcloud

```bash
 $ nano permisions.sh
```
In it , add the following 

```bash

 #!/bin/bash
ncpath='/var/www/nextcloud'
htuser='www-data'
htgroup='www-data'
rootuser='root'

printf "Creating possible missing Directories\n"
mkdir -p $ncpath/data
mkdir -p $ncpath/assets
mkdir -p $ncpath/updater

printf "chmod Files and Directories\n"
find ${ncpath}/ -type f -print0 | xargs -0 chmod 0640
find ${ncpath}/ -type d -print0 | xargs -0 chmod 0750

printf "chown Directories\n"
chown -R ${rootuser}:${htgroup} ${ncpath}/
chown -R ${htuser}:${htgroup} ${ncpath}/apps/
chown -R ${htuser}:${htgroup} ${ncpath}/assets/
chown -R ${htuser}:${htgroup} ${ncpath}/config/
chown -R ${htuser}:${htgroup} ${ncpath}/data/
chown -R ${htuser}:${htgroup} ${ncpath}/themes/
chown -R ${htuser}:${htgroup} ${ncpath}/updater/

chmod +x ${ncpath}/occ

printf "chmod/chown .htaccess\n"
if [ -f ${ncpath}/.htaccess ]
 then
  chmod 0644 ${ncpath}/.htaccess
  chown ${rootuser}:${htgroup} ${ncpath}/.htaccess
fi
if [ -f ${ncpath}/data/.htaccess ]
 then
  chmod 0644 ${ncpath}/data/.htaccess
  chown ${rootuser}:${htgroup} ${ncpath}/data/.htaccess
fi

```

Make that file executable and run the file

```bash
 $ chmod +x permisions.sh
 $ ./permisions.sh
```
Change the file ```/etc/apache2/sites-available/000-default.conf``` to look like this

```bash
 <VirtualHost *:80>
 
 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/nextcloud
 
    <Directory “/var/www/html/nextcloud”>
        Options Indexes FollowSymLinks
        AllowOverride All
        Order allow,deny
        allow from all
    </Directory>
 
 
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
 
</VirtualHost>
```

Save and restart apache

```bash
 $ systemctl restart apache2
```

Finnally configure database.


```bash
 $ sudo mysql -u root -p
 CREATE DATABASE nextcloud_db;
 CREATE USER cooluser@localhost IDENTIFIED BY 'coolpassword';
 GRANT ALL PRIVILEGES ON nextcloud_db.* TO cooluser@localhost;
 EXIT
```

Access Nextcloud on http://localhost or http://DOMAIN

NB:Use similar credentions for database

 Database User = cooluser

 Database password = coolpassword

 Database name = nextcloud_db

 localhost = localhost










