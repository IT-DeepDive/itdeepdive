---
title: "How to: Install TFTP Server on Centos 6"
date: 2020-04-16
description: "How to: Install, Configure and Manage a TFTP Server on Centos 6"
#featured: true
toc: false
codeMaxLines: 10
codeLineNumbers: false
figurePositionShow: true
categories:
- How To
tags:
- linux
- centos
- tftp
---

Recently I had to troubleshoot a TFTP server that was part of a vSphere AutoDeploy environment.
In the attempt to reproduce the issue in my lab, I deployed and configured a TFTP Server running on Centos 6. Here is what I did.


**Step 1** - Update the server: 
```
yum update && yum upgrade -y
```

**Step 2** - Install the following packages 
```
yum install -y tftp tftp-server tcpdump
```

*[Optional]* If the server is not connected to internet, mount the iso DVD and install from local source:
```
mount /dev/cdrom /media/cdrom
```
And install the packages from the local repository (CDROM): 
```
yum --disablerepo=\* --enablerepo=c6-media install tftp tftp-server tcpdump
```

**Step 3** - TFTP in CentOS 6.x runs under the xinetd Super Daemon. The configuration file is in `/etc/xinetd.d/tftp`\
![tftp settings under xinetd](/post/2020/04/20200416-010826.png)

> Change `disable = yes` to `disable = no`

**Step 4** - Start xinetd service:
```
service xinetd start
```

Check if the server is waiting for connections on port udp 69:
```
netstat -tulpn
```
![netstat -tulpn](/post/2020/04/20200416-011258.png)

Don't forget to check the firewall: 
```
iptables -L INPUT -v
```
![iptables rules](/post/2020/04/20200416-011544.png)

If needed, add a rule to allow traffic on **udp port 69**: 
```
iptables -I INPUT -p udp -m state --state NEW -m udp --dport 69 -j ACCEPT
```
![iptables udp 69 allowed](/post/2020/04/20200416-011818.png)

Logs are written in `/var/log/messages`.

Use `tcpdump` to capture incoming traffic on port 69:
```
tcpdump -nei eth0 udp port 69
```
Enjoy!