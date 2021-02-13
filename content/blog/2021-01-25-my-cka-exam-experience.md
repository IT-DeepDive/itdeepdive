---
title: "My CKA Exam Experience"
date: 2021-01-25
description: "How did I prepare and pass the Certified Kubernetes Administrator (CKA) exam."
featured: true
featureImage: "/images/cka-certificate.jpg"
thumbnail: /thumbnails/cka-200x150.png
tags:
- Kubernetes
- k8s
- Certification
- CKA
---

A few months back, when I was working as a Sr. NV Engineer at [VMware](https://www.vmware.com/) I was given the opportunity to attend a Course held by [GURUTEAM](https://www.guruteamirl.com/): [Kubernetes Administration Certification](https://www.guruteamirl.com/training/kubernetes-administration-certification.490.html).

It lasted four days (_obviously remote!_) and the instructor covered almost all the topics required for the **CKA Certification**. Even though he was very knowledgeable, friendly and engaging, four days to cover lectures and practical tests is just not enough to get you ready for the actual exam. However it teaches you the basics and gives you an idea of what you have to do to get up to speed.

I spent the following months studying and practicing on my spare time and I was also involved, as part of my daily job, in troubleshooting Customers network issues related to **Kubernetes** in conjunction with **NSX**, specifically **NCP** which happens to be the [Network Plug-in](https://docs.vmware.com/en/VMware-NSX-T-Data-Center/3.1/ncp-kubernetes/GUID-52A92986-0FDF-43A5-A7BB-C037889F7559.html) (or [CNI](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/)) that integrates Kubernetes networking into NSX-T.

A very useful resource worth mentioning is the [Certified Kubernetes Administrator (CKA) with Practice Tests](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/) Course, on [Udemy](https://www.udemy.com/), not just for the Course itself, but specially for the many practice tests the course gives you access to via [KodeKloud](https://kodekloud.com/).

The Exam is completely practical, so you have to be very confident with objects creation (pods, deployments, namespaces, etc) via `kubectl`. Some tips which may help:
- Don't waste to much time creating too many aliases; use autocomplete instead
- Use the imperative commands to create objects as much as you can, in order to avoid typos and formatting issues with the `yaml` syntax
- When you can't use imperative commands, try re-using the previously created `yaml` files.
- Familiarise with the [Official Kubernetes Documentation](https://kubernetes.io/docs/home/) because it has great resources and examples that you can use during the exam.
- Be sure to check out the [kubectl Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/) at https://kubernetes.io/
- Review the [Frequently Asked Questions: CKA and CKAD & CKS](https://docs.linuxfoundation.org/tc-docs/certification/faq-cka-ckad-cks)

The passing score is **66%** and the tasks have different weight indicated in the description so it's quite easy to figure out your score at any given time. Do the math so that you can pick and choose the tasks you need completed in order to be successful.

The exam is remote/proctored and I find it very handy because you can schedule it anytime you want.
I did it mine on a Saturday morning a 7am and I got the results on Sunday night at around 9pm.

### CKA Exam format
- Online
- Duration of Exam 2 hours
- 17 Tasks in my case, but I think it may vary
- Certification Valid for 3 Years
- 1 Free Retake

### Domain & Competencies
- Storage 10%
- Troubleshooting 30%
- Workloads & Scheduling 15%
- Cluster Architecture, Installation & Configuration 25%
- Services & Networking 20%

More information [here](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/).

## CKA Brain Dump Questions
1. **Overall, did I find the exam difficult?**\
   _I'd say "fair enough". It really challenges your skills, so you have to be prepared!_
1. **Was I caught for time?**\
   _Yes! 120 minutes for 17 tasks to complete doesn't give you too much time to think and research. You have to know what you are doing as soon as you read the task._
1. **What topics would I advise people to know really well?**
   - _Basic components and how they interact with each other is critical for troubleshooting issues_
   - _Ingress_
   - _Network Policies_
   - _Etcd DB backup and **restore**_
   - _JSONPath for gathering data_
1. **What topics was I surprised to see?**\
   _There was a question regarding CPU usage for pods which implied the use of `kubectl top`, so you have to make sure the [Metrics Server](https://github.com/kubernetes-sigs/metrics-server) is installed and running. Heads up!_
1. **Test Center or Remote/proctored?**\
   _Remote / Proctored. You can buy and schedule the exam from [The Linux Foundation](https://training.linuxfoundation.org/certification/certified-kubernetes-administrator-cka/)._
1. **Any additional resources to prepare for the Exam?**
   - [Official Kubernetes Documentation](https://kubernetes.io/docs/home/)
   - [Kubernetes Fundamentals (LFS258)](hhttps://training.linuxfoundation.org/training/kubernetes-fundamentals/) from [The Linux Foundation](https://www.linuxfoundation.org/)
   - [Certified Kubernetes Administrator (CKA) with Practice Tests](https://www.udemy.com/course/certified-kubernetes-administrator-with-practice-tests/) on [Udemy](https://www.udemy.com/)

Good Luck!

[![](/thumbnails/cka-200x150.png)](https://www.youracclaim.com/badges/ac5c35ec-1b1c-4656-b724-054c46f49fda/public_url)\
[CKA: Certified Kubernetes Administrator](https://www.youracclaim.com/badges/ac5c35ec-1b1c-4656-b724-054c46f49fda/public_url)\
Issued by [The Linux Foundation](https://www.youracclaim.com/organizations/the-linux-foundation/badges)
