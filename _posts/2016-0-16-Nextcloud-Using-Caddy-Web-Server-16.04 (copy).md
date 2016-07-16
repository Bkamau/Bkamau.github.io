---
layout: post
title: Nextcloud Using Caddy Web Server
subtitle: Easy quick setup
---

If u dont know Caddy web server, <a href="https://caddyserver.com/">visit this link</a>

If you dont have mysql installed, install it 

```bash
 $ sudo apt update && apt -y install mysql-server
```

Create database for nextcloud

```bash
 $ > CREATE DATABASE nextcloud;
 $ > GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost' IDENTIFIED BY 'nextcloudPassword';
 $ > FLUSH PRIVILEGES;
 $ > \q
```

Install php and needed modules

```bash
 $ sudo apt-get -y install php-fpm php-cli php-json php-curl php-imap php-gd php-mysql php-xml php-zip php-intl php-mcrypt php-imagick php-mbstring
```

Tweak some php settings

```bash
 $
 $ sudo sed -i "s/memory_limit = .*/memory_limit = 512M/" /etc/php/7.0/fpm/php.ini
 $ sudo sed -i "s/;date.timezone.*/date.timezone = UTC/" /etc/php/7.0/fpm/php.ini
 $ sudo sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=1/" /etc/php/7.0/fpm/php.ini
 $ sudo sed -i "s/upload_max_filesize = .*/upload_max_filesize = 200M/" /etc/php/7.0/fpm/php.ini
 $ sudo sed -i "s/post_max_size = .*/post_max_size = 200M/" /etc/php/7.0/fpm/php.ini
```

Accept PHP-FPM request on a TCP socket instead of a Unix by changing by opening the file

```
/etc/php/7.0/fpm/pool.d/www.conf
```

and replace the line 

```
listen = /run/php/php7.0-fpm.sock
```

with 

```
listen = 127.0.0.1:9000
```
Then

```bash
 $ sudo systemctl restart php7.0-fpm
```

Cd to /tmp, grab nextcloud, extract it, move it to /www

```bash
 $ cd /tmp && wget https://download.nextcloud.com/server/releases/nextcloud-9.0.51.tar.bz2
 $ tar -vxjf nextcloud-9.0.51.tar.bz2 && mv nextcloud /var/www/

```
Lets have our data in a different location

```bash
 $ mkdir /mnt/nextcloud /mnt/nextcloud/data
 $ sudo chown -R www-data: /mnt/nextcloud

```

Install caddy

```bash
 $ curl https://getcaddy.com | bash
```

Create file ``` /etc/systemd/system/caddy.service
``` and add into it

```bash
[Unit]
Description=Caddy HTTP/2 web server %I
Documentation=https://caddyserver.com/docs
After=network-online.target
Wants=network-online.target
Wants=systemd-networkd-wait-online.service

[Service]
; run user and group for caddy
User=root
Group=root
ExecStart=/usr/bin/caddy -agree=true -conf=/etc/caddy/Caddyfile
Restart=on-failure

; create a private temp folder that is not shared with other processes
PrivateTmp=true

; limit the number of file descriptors, see `man systemd.exec` for more limit settings
LimitNOFILE=8192

[Install]
WantedBy=multi-user.target
```

Save and create Caddyfile

```bash
 $ mkdir /etc/caddy
 $ nano /etc/caddy/Caddyfile
```
In it add this 

```bash

nextcloud.example.com {

    root /var/www/nextcloud
   
    fastcgi / 127.0.0.1:9000 php {
            env PATH /bin
    }

    rewrite {
        r ^/index.php/.*$
        to /index.php?{query}
    }

    # client support (e.g. os x calendar / contacts)
    redir /.well-known/carddav /remote.php/carddav 301
    redir /.well-known/caldav /remote.php/caldav 301

    # remove trailing / as it causes errors with php-fpm
    rewrite {
        r ^/remote.php/(webdav|caldav|carddav)(\/?)$
        to /remote.php/{1}
    }

    rewrite {
        r ^/remote.php/(webdav|caldav|carddav)/(.+)(\/?)$
        to /remote.php/{1}/{2}
    }

    # .htacces / data / config / ... shouldn't be accessible from outside
    rewrite {
        r  ^/(?:\.htaccess|data|config|db_structure\.xml|README)
        status 403
    }

    header / Strict-Transport-Security "15768000"

}

```
Replace domain accordingly.

Save and start caddy

```bash
 $ systemctl start caddy.service
 $ systemctl enable caddy.service
```

You are done.

Have fun.



