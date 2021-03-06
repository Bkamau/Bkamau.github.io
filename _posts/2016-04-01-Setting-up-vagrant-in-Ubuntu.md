---
layout: post
title: Vagrant on Ubuntu
subtitle: Easy quick setup
---
Vagrant is a virtual environment that comes in handy for app developers as well as server administrators. 

You can spin up quite as many virtual Linux boxes as your host machine can handle, configure them as you want and later destroy them when you are done with your project.

This makes sure that your host machine remains clean and stable. Another advantage of using vagrant is many developers for example, regardless of the operating systems they are running can spin up one environment and do their project on it.

This eliminates the all too famous tag " .. but it was working on my machine.."


In this tutorial, we shall
- set up vagrant environment on Linux
- configure it for Django development
- Create a sleek blog
- Deploy the project on Amazon WS

To start us off, head over to <a href="https://www.vagrantup.com/downloads.html" target="_blank">vagrant download page</a> </a> and install vagrant based on your architecture. 

Next, Install oracle virtual box from  <a href="https://www.virtualbox.org/wiki/Downloads" target="_blank">this site</a>. It is possible to use other providers like VM Players but for now lets go with virtual box.

Choosing a virtual box is the next thing. you can pick any box you want from sites like

<a href="http://www.vagrantbox.es" target="_blank">http://www.vagrantbox.es</a>
<a href="https://atlas.hashicorp.com/boxes/search" target="_blank">https://atlas.hashicorp.com/boxes/search</a>

For this tutorial and concistency sake i shall use Ubuntu 15.04 64bit which can be found <a href="https://atlas.hashicorp.com/boxes/search?utf8=%E2%9C%93&sort=&provider=&q=ubuntu+" target="_blank">here</a>. Open terminal and paste the following commands.

```bash
$ mkdir Vagrant
$ cd Vagrant
$ vagrant box add ubuntu/vivid64
$ vagrant init ubuntu/vivid64
```

This will place a vagrant file in the vagrant directory we just created. Most of the lines in this file are commented. Uncomment the following two lines by removing the hash tag 

```bash
 config.vm.network "forwarded_port", guest: 80, host: 8080
 config.vm.network "private_network", ip: "192.168.33.10"
```

Next, Add another port line below the one we have uncommented with variables, guest:8000, host:8000

Change the IP address on the other line you have uncommented to anything of your choice, i will use 33.33.33.33.
We shall use this to create a fake domain name.

By now your configurations should look like this

```bash
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network "forwarded_port", guest: 8000, host: 8000

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: "33.33.33.33"
```

Save the file and on the terminal, issue the command 

```bash
  $ vagrant up
```

Now you have a running Linux virtual box. SSH into the virtual machine using this command

```bash
  $ vagrant ssh
```

And the result should be 

```bash
	$ vagrant ssh
	Welcome to Ubuntu 15.04 (GNU/Linux 3.19.0-42-generic x86_64)

	 * Documentation:  https://help.ubuntu.com/

	  System information as of Sat Jan  2 15:11:34 UTC 2016

	  System load:  0.14              Processes:           74
	  Usage of /:   3.4% of 38.80GB   Users logged in:     0
	  Memory usage: 37%               IP address for eth0: 10.0.2.15
	  Swap usage:   0%                IP address for eth1: 33.33.33.33

	  Graph this data and manage this system at:
	    https://landscape.canonical.com/

	  Get cloud support with Ubuntu Advantage Cloud Guest:
	    http://www.ubuntu.com/business/services/cloud

	0 packages can be updated.
	0 updates are security updates.

	New release '15.10' available.
	Run 'do-release-upgrade' to upgrade to it.


	vagrant@vagrant-ubuntu-vivid-64:~$ 
```
    

There, you have a virtual environment running. Notice the terminal syntax has changed.

By using synced folders, Vagrant will automatically sync your files to and from the guest machine. By default, vagrant shares the vagrant directory we created(the one with the vagrant file)

On the virtual machine, (NOT your host machine), cd to the vagrant folder and touch a file, that is

```bash
  $ cd /vagrant
  $ touch hello
```
On your host machine, in the vagrant folder we created, is a file hello. Therefore this folder syncs with the vagrant folder in our virtual machine.

Finally, lets install a local server in the virtual box and create a fake domain name to access it. For the local server , i shall install Apache, you can use Nginx if u prefer.

```bash
$ sudo apt-get update
$ sudo apt-get install -y apache2  
$ sudo service apache2 restart
```
Now on your host machine, if you go to localhost or 127.0.0.1 from your browser , nothing is showing.

But if you use the IP address we specified in the vagrant file, you should be able to reach the server, so type 33.33.33.33 (or any other IP address you specified, if not sure, open the vagrant file and confirm) on your web browser and you should To be able to reach the server running on your virtual box.

With vagrant, it is possible to setup fake domain names instead of using numbers on your browser. From the command line, type

```bash
$ exit
```
this will take you back to your host machine.

Enter the command 

```bash
 $ sudo gedit /etc/hosts 
```

Add the address we specified together with a fake domain. If for example your project is example.com , its good to use dev.example.com . 

Be careful not to provide a real working domain, otherwise you will not be able to reach it. Also, DONT change anything else in that file apart from that one line we are adding. Your file should look similar(maybe not exactly) to this 


```bash

	127.0.0.1	localhost
	127.0.1.1	spike

	# The following lines are desirable for IPv6 capable hosts
	::1     ip6-localhost ip6-loopback
	fe00::0 ip6-localnet
	ff00::0 ip6-mcastprefix
	ff02::1 ip6-allnodes
	ff02::2 ip6-allrouters

	33.33.33.33 dev.example.com
```



Save and close the file, from you browser navigate to dev.example.com and again you should be able to reach the server running on your spinning virtual box.

Other vagrant commands that you can use include

vagrant suspend -  To suspend a running box
vagrant reload - reload the box after editing any configurations
vagrant destroy- destroy the box and remove any traces it existed

In our next tutorial, we shall set up Django environment in the created virtual box.











