---
title: "Getting Started Docker Containers"
date: 2020-10-19
description: "Getting Started Docker Containers."
featured: true
draft: true
toc: false
codeMaxLines: 10
codeLineNumbers: false
categories:
- Tutorial
series:
- Kubernetes
tags:
- ubuntu
- docker
- containers
- hyper-v
---

In [the previous article](../kubernetes-multi-node-cluster-with-multipass) we discussed how to deploy a 3 nodes Kubernetes Cluster on **Ubuntu Server 18.04** using Multipass. in this article we'll dive into Docker Containers common usage.

## Docker Cheat Sheet
Command | Result
-|-
`docker pull <image>` | Download an image
`docker run <image>` | Download and run the image
`docker images` | List all downloaded images
`docker run -it <image>` | Run an image an allocate a TTY
`docker run -d <image>` | Run container in background and print container ID
`docker ps` | List containers
`docker start <id>` | Start one or more stopped containers
`docker stop <id>` | Stop one or more running containers
`docker rm <id>` | Remove one or more containers
`docker system prune` | Remove unused data
