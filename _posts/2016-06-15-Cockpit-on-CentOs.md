---
layout: post
title: Install Cockpit in CentOs
subtitle: Easy quick setup
---

Have cockpit running in your Centos server with this simple steps.

Install cockpit

```bash
 $ yum install cockpit
```


Enable Cockpit service.

```bash
 $ systemctl enable cockpit.socket
 $ ln -s '/usr/lib/systemd/system/cockpit.socket' '/etc/systemd/system/sockets.target.wants/cockpit.socket'

```

Add Cockpit to the list of trusted services in FirewallD.

```bash
 $ firewall-cmd --permanent --zone=public --add-service=cockpit
success

 $ firewall-cmd --reload
success

 $ firewall-cmd --list-services
cockpit dhcpv6-client ssh
```

Start Cockpit socket.

```bash
 $ systemctl start cockpit.socket
```

You will need another step before you start using Cockpit. We need to modify the cockpit service file to disable SSL as there seems to be some issue with this. For this, edit the file /usr/lib/systemd/system/cockpit.service and change the line starting with ExecStart to the following:

```bash
ExecStart=/usr/libexec/cockpit-ws --no-tls
```
NB: This is not recommended on a production environment.

Reload systemd.

```bash
 $ systemctl daemon-reload
```
Restart Cockpit.

```bash
 $ systemctl restart cockpit
```

Access Cockpit web interface at http://DOMAIN:9090/

Have fun.
