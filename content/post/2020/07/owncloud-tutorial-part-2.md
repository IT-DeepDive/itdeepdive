---
title: "ownCloud Tutorial Part 2: Configure custom URL and HTTP port"
date: 2020-07-18 
description: "ownCloud Tutorial Part 2: Configure custom URL and HTTP port" # Description used for search engine.
featured: false # Sets if post is a featured post, making appear on the home page side bar.
toc: true # Controls if a table of contents should be generated for first-level links automatically.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Tutorial
series:
  - ownCloud
tags:
  - linux
  - owncloud
# comment: false # Disable comment if false.
---

# Configure a custom url and http port
In this chapter we'll discuss how to configure Apache web server to redirect the homepage from `http://owncloud_ip/owncloud/` to `http://owncloud_ip:custom_port`.  
In this example we are going to be using the IP address `192.168.0.105` and tcp port `8080` (`http://192.168.0.105:8080`).  

## Configure httpd to listen on port 8080
Backup the current `/etc/httpd/conf/httpd.conf` and change `Listen 80` to `Listen 192.168.0.105:8080`.
```shi
cd /etc/httpd/conf/ && cp -p httpd.conf httpd.conf.bck && sed -i 's/^Listen 80$/Listen 192.168.0.105:8080/' httpd.conf
```
Save and close the file.

## Create a VirtualHost to redirect the homepage
Paste the following on your terminal as root in order to create the VirtualHost:

```sh
vhost="/etc/httpd/conf.d/owncloud.conf"
/bin/cat <<EOF >$vhost
<VirtualHost 192.168.0.105:8080>
  ServerName owncloud.mylab.local
  DocumentRoot /var/www/html/owncloud
  ErrorLog logs/owncloud_error_log
  CustomLog logs/owncloud_access_log combined
</VirtualHost>
EOF

```

You may want to check if the syntax of the configuration is correct with the following command:
```sh
apachectl configtest
```
![vhost verification](/post/2020/07/img/02-vhost-01.png)

Now restart Apache to make these changes take effect.

```sh
systemctl restart httpd
```

## Allow port 8080 through the Firewall
```sh
firewall-cmd --permanent --zone=public --add-port=8080/tcp
firewall-cmd --reload
```

Now you should be able to access your ownCloud server pointing your browser to `http://owncloud_ip:8080`
![Completed](/post/2020/07/img/02-vhost-02.png)

