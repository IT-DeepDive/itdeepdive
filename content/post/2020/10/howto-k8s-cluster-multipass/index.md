---
title: "Kubernetes Multi-Node Cluster with Multipass"
date: 2020-10-12
description: "How to create a Kubernetes Cluster leveraging Multipass"
featured: true
draft: true
toc: false
codeMaxLines: 10
codeLineNumbers: false
categories:
- How To
series:
- Kubernetes
tags:
- multipass
- ubuntu
- k8s
- hyper-v
---

In this short article I will explain how to create a 3 nodes Kubernetes cluster on your Mac leveraging [Multipass](https://multipass.run/).
> "Multipass provides a command line interface to launch, manage and generally fiddle about with instances of Linux. The downloading of a minty-fresh image takes a matter of seconds, and within minutes a VM can be up and running."\
<i>The Register</i>

## Deploy the 3 Kubernetes nodes via Multipass
Let's list what images are available to deploy:
```
multipass find
```
![multipass list](img/multipass-find.jpg)

For my Kubernetes Cluster I'll deploy 3 nodes (One master node and two workers) using `Ubuntu 18.04 LTS` as follows:

- name: **k8s-master-1**
- CPU: 2
- Disk: 32 Gb
- Memory: 2 Gb
- image: 18.04

```
multipass launch -c 2 -d 32G -m 2G -n k8s-master-1 18.04
```

- name: **k8s-worker-1**
- CPU: 2
- Disk: 32 Gb
- Memory: 2 Gb
- image: 18.04

```
multipass launch -c 2 -d 32G -m 2G -n k8s-worker-1 18.04
```

- name: **k8s-worker-2**
- CPU: 2
- Disk: 32 Gb
- Memory: 2 Gb
- image: 18.04

```
multipass launch -c 2 -d 32G -m 2G -n k8s-controller-2 18.04
```

A `multipass list` will reveal whether the 3 instances have been deployed and what's their state:
![multipass list](img/multipass-list.jpg)

From now on, to access these server's shells, we will use the command
```
multipass shell <instance_name>
```
![Ubuntu shell](img/multipass-shell.jpeg)

## Install the CRI
In order to run Containers we need a [Container Runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/). For this excercise we'll use [Docker](https://www.docker.com/).

On each of your nodes, install Docker CE.

For your convenience you can use the following script:

```bash
#!/bin/bash

# (Install Docker CE)
## Set up the repository:
### Install packages to allow apt to use a repository over HTTPS
sudo apt-get update && sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common gnupg2

# Add Docker's official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key --keyring /etc/apt/trusted.gpg.d/docker.gpg add -

# Add the Docker apt repository:
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# Install Docker CE
sudo apt-get update && sudo apt-get install -y containerd.io=1.2.13-2 docker-ce=5:19.03.11~3-0~ubuntu-$(lsb_release -cs) docker-ce-cli=5:19.03.11~3-0~ubuntu-$(lsb_release -cs)
```
Feel free to save the file as `install-docker.sh` and run in on your 3 nodes.




















### Next steps
- Check out [Getting started with Containers](../getting-started-with-containers)
