---
title: "Install and Configure GNS3 with VirtualBox on macOS Catalina"
date: 2021-02-13
description: "In this article, we will illustrate how to configure GNS3 2.2.18 and VirtualBox VM on a macOS running Catalina 10.15.7."
featured: true
thumbnail: "/thumbnails/gns3-plus-mac-200x150.png"
toc: false
codeMaxLines: 10
codeLineNumbers: false
categories:
- How to
series:
- GNS3
tags:
- Networking
- Linux
- Mac
- VirtualBox
---

In this article, I'll show you how to install and configure GNS3 on a Mac Catalina.
We will focus on these specific version, GAs at the time of writing but hopefully the same instructions might be applicable for slightly different versions:
- GNS3 version 2.2.18
- macOS Catalina 10.15.7

## Step 1 - Download GNS3 and GNS3 VM
Head over to [GNS3 software page](https://gns3.com/software/download) and download macOS version 2.2.18.
If you don't have an account, you will have to create one!
For optimal performance, also [download the GNS3 VM](https://gns3.com/software/download-vm).
In this article I will be using [VirtualBox](https://www.virtualbox.org/wiki/Downloads), so fell free to download and install it as well if you'd like to follow along.

## Step 2 - Installation
The installation is pretty straightforward. Just drag GNS3 into your application folder and it's done:\
![GNS3 Installation](/images/20210213-gns3-install.jpg "GNS3 Installation")

Now it's time to extract the GNS3 VM and import it into VirtualBox. For mac users, just double clic `GNS3.VM.VirtualBox.2.2.18.zip` you just downloaded and a new folder `GNS3 VM` will be automatically created for you containing the extracted files.
From VirtualBox UI select Import and browse for the file `GNS VM.ovf` inside the `GNS3 VM` folder.\
![Importing GNS3 VM](/images/20210213-gns3vm-import.jpg "Importing GNS3 VM"). On the next screen click on `import` and you should be good to go. macOS might warn you about permission to monitor files and control the system from your terminal. These permissions are needed from the GNS3 application to control the GNS3 VM (start, stop, configure interfaces, cpu, ram, etc.)\
![GNS3 VM ready](/images/20210213-gns3vm-ready.jpg "GNS3 VM ready")

## Step 3 - Initial configuration
Start the GNS3 VM and wait for the boot process to complete. You should see a screen like this:\
![GNS3 VM started](/images/20210213-gns3vm-started.jpg "GNS3 VM started")
Make sure `KVM support available` is set to `True` and take note of the IP; in this case `192.168.56.104`.
Try and ping the GNS3 VM IP from macOS terminal. If ping does not work check if the `vboxnet0` has been assigned the correct network, from the `VirtualBox menu` -> `File` -> `Host Network Manager...`:\
![Host Network Manager](/images/20210213-host-network-manager.jpg "Host Network Manager").
When you are able to ping the GNS3 VM it's time to open GNS3 and check its `Preferences`.
At first execution, allow root permissions for uBridge to interact with network interfaces:\
![uBridge root permissions](/images/20210213-ubridge-root-permissions.jpg "uBridge root permissions")
In `General` you can set your own preferences for where to store your projects etc. In order to be sure networking is going to work properly, select `Server` from the left menu and verify `Enable local server` is flagged and `Host binding:` matches your GNS3 VM subnet. You can leave the other settings as default unless you have a specific reason to customise them:\
![GNS3 Server preferences](/images/20210213-gns3-server-preferences.jpg "GNS3 Server preferences")
In `GNS3 VM preferences` make sure `Enable the GNS3 VM` is enabled and the correct `Virtualization engine` is selected. If you can't see the correct `VM name` try refershing the settings. You should see something like this:\
![GNS3 VM preferences](/images/20210213-gns3vm-preferences.jpg "GNS3 VM preferences")

## Step 4 - Testing your first project
Okay, now let's have some fun!
1. Create a new project and from the left pane `Browse all devices` icon drag the `NAT` Cloud, a switch and 2 `VPCS`.
1. Connect the `NAT` cloud and the 2 `VPCS` to the `Switch`.
1. Open a console for both `VPCS` and obtain an IP from the DHCP with the command `ip dhcp`
1. After obtaining the IP, from `PCI` try pinging `PC2` and from `PC2` try pinging an external DNS, like `1.1.1.1`.
![Testing connectivity](/images/20210213-my-first-project.jpg "Testing connectivity")
