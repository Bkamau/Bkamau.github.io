This is the second part of our tutorial, First part can be found  <a href="http://benson.fi/index.php/2016/01/04/3356/">here </a>

Continuing from where we left, power up your virtual box and ssh into in case you had destroyed it.

Begin by updating the source list

[bash light="true"] $ sudo apt-get update[/bash]

<!--more-->


Next, install pip and virtual environment (Its important to use virtual environment in case you need to freeze requirements, however, its optional)

[bash light="true"]$ sudo apt-get install -y python-pip[/bash]

[bash light="true"]$ sudo pip install virtualenv[/bash]

Next cd into the virtual box vagrant folder, Create a folder called Mysite and within it create a virtual environment, activate it,install Django and start a django project, in that order.

[bash light="true"]
$ cd /vagrant
$ mkdir Mysite
$ cd Mysite
$ virtualenv venv
$ source venv/bin/activate
$ pip install django
$ django-admin startproject mysite
[/bash]

More info on virtual environment can be found here  <a href="https://virtualenv.readthedocs.org/en/latest/" target="_blank">https://virtualenv.readthedocs.org/en/latest/</a>
More info on Django can be found here <a href="https://www.djangoproject.com/" target="_blank">https://www.djangoproject.com/</a>

Navigate inside the project file created and run the project

[bash light="true"]
$ cd mysite
$ python manage.py runserver [::]:8000
[/bash]

On your host machine, from the browser navigate to http://dev.example.com:8000/ and you Django app should be running.

NOTE, This is a continuation of the previous tutorial, please refer to it to see how we configured the domain and addresses.

In our next tutorial, we shall connect Pycharm on our host machine to the project and interpreter in the virtual box
