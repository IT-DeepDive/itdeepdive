---
title: "Kubernets Multi-Node Cluster with Multipass"
date: 2020-10-12
description: "Article description."
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

In this short article I will explain how to create a 3 nodes Kubernetes cluster on your Mac leveraging [Ubuntu Multipass](https://multipass.run/).
> "Multipass provides a command line interface to launch, manage and generally fiddle about with instances of Linux. The downloading of a minty-fresh image takes a matter of seconds, and within minutes a VM can be up and running."\
<i>The Register</i>

## Deploy the 3 Kubernetes nodes via Multipass
Let's list what images are available to deploy:
```
multipass find
```
![multipass list](img/20201123-130638.png)

For my Kubernetes Cluster I'll deploy 3 nodes (One master node and two controllers) using `Ubuntu 20.04 LTS` as follows:

- name: **k8s-master-1**
- CPU: 2
- Disk: 32 Gb
- Memory: 2 Gb
- image: 20.04

```
multipass launch -c 2 -d 32G -m 2G -n k8s-master-1 20.04
```

- name: **k8s-controller-1**
- CPU: 2
- Disk: 32 Gb
- Memory: 2 Gb
- image: 20.04

```
multipass launch -c 2 -d 32G -m 2G -n k8s-controller-1 20.04
```

- name: **k8s-controller-2**
- CPU: 2
- Disk: 32 Gb
- Memory: 2 Gb
- image: 20.04

```
multipass launch -c 2 -d 32G -m 2G -n k8s-controller-2 20.04
```

A `multipass list` will reveal whether the 3 instances have been deployed and what's their state:
![multipass list](img/20201123-133200.png)

We can also veryfy the same information from Hyper-V Manager UI:
![Hyper-V Manager UI](img/20201123-133607.png)

From now on, to access these server's shells, we will use the command
```
multipass shell <instance_name>
```
![Ubuntu shell](img/20201123-134636.png)

### Next steps
- Check out [Getting started with Containers](../getting-started-with-containers)
