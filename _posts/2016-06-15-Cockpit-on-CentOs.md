Have cockpit running in your Centos server with this simple steps.

Clone the GutHub repository.

git clone https://github.com/baude/sig-atomic-buildscripts
Copy virt7-testing.repo file to /etc/yum.repos.d and install Cockpit.
~~~
yum install cockpit
~~~

Enable Cockpit service.

[root@webtest ~]# systemctl enable cockpit.socket
ln -s '/usr/lib/systemd/system/cockpit.socket' '/etc/systemd/system/sockets.target.wants/cockpit.socket'
[root@webtest ~]#
Add Cockpit to the list of trusted services in FirewallD.

[root@webtest ~]# firewall-cmd --permanent --zone=public --add-service=cockpit
success
[root@webtest ~]#
[root@webtest ~]# firewall-cmd --reload
success
[root@webtest ~]#
[root@webtest ~]# firewall-cmd --list-services
cockpit dhcpv6-client ssh
[root@webtest ~]#
Start Cockpit socket.

systemctl start cockpit.socket
Do no try to access Cockpit yet, there is an issue about running Cockpit on stock CentOS/RHEL 7. To be able to start it we need first to modify the service file to disable SSL.Edit file /usr/lib/systemd/system/cockpit.service and modify ExecStart line to look like this.

ExecStart=/usr/libexec/cockpit-ws --no-tls
I know this procedure will invalidate Cockpit for a production environment in RHEL7 at least for now but this is for my lab environment and I can live with it.

Reload systemd.

systemctl daemon-reload
Restart Cockpit.

systemctl restart cockpit
Access Cockpit web interface, login as root and have fun :-)
