---
layout: post
title: Parse Platform on Debian
subtitle: Easy quick setup
---

In order to run parse server, we need to install both NodeJs and mondoDb.

We install NodeJs depedancies first.

```bash
 $ sudo apt-get update  && apt-get upgrade -y
 $ curl -sL https://deb.nodesource.com/setup_6.x -o nodesource_setup.sh
 $ sudo bash ./nodesource_setup.sh
```

And then install Nodejs,build-essential and git.

```bash
 $ sudo apt-get install -y nodejs build-essential git
```

Install process manager

```bash
 $ npm install -g pm2
```

Next, install mongodb. 

```bash
 
 $ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv EA312927
 $ echo "deb http://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/3.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-3.2.list
 $ sudo apt-get update
 $ sudo apt-get install -y mongodb-org
 $ sudo service mongod start
```

Now we have depedancies in place, next we shall clone a parse server app, add it to forever service and start it.

```bash
 $ git clone https://github.com/ParsePlatform/parse-server-example.git
 $ cd ~/parse-server-example
 $ npm install
 $ sudo pm2 start index.js
```
Head over to http://localhost:1337/test ( http://DOMAIN:1337/test ) and test the app.

We shall do the same with the parse dashaboard

```bash
 $ git clone https://github.com/ParsePlatform/parse-dashboard.git
 $ cd parse-dashboard
 $ npm install
 $ sudo forever-service install parse-dashboard --script ./Parse-Dashboard/index.js --scriptOptions " allowInsecureHTTP"
 $ sudo service parse-dashboard start
```
NB: If you have a private server connection, remove the tag  "allowInsecureHTTP"

Edit file ./parse-dashboard/Parse-Dashboard/parse-dashboard-config.json with the parse server and parse user.

Thats it. Your app is running at http://localhost:1337/parse

Your dashboard is running at  http://localhost:4040

Have fun.















