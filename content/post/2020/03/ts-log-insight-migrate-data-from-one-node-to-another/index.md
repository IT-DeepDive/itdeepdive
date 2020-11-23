---
title: "Log Insight: Migrate data from one node to another"
date: 2020-03-31
description: "Log Insight: Migrate data from one node to another."
#featured: true
toc: false
codeMaxLines: 10
codeLineNumbers: false
categories:
- Troubleshooting
series:
- vRealize Log Insight
tags:
- log insight
- vmware
---

There are some occasions when you may want to migrate the  data between nodes inside a vRealize Log Insight Cluster. An example could be if one of the nodes in the cluster is broken for any reasons (bugs, application problems, accidental deletion of vital configuration files, file corruption, etc.)

> **Disclaimer:** the following troubleshooting steps may cause data loss. Proceed at your own risk *only* you know what you are doing.

Restore log events from the old Cluster to the new Cluster.
1. Stop the loginsight service on the old Cluster on every node:
   ```
   service loginsight stop
   ```
1. Move the logs from the old nodes to the new corresponding nodes (old 1 to new 1, old 2 to new 2, ecc):
   ```
   scp -r /storage/core/loginsight/cidata/store <newnode>:/storage/core/loginsight/cidata 
   scp -r /storage/core/loginsight/cidata/store/*-*-*-*-* <newnode>:/storage/core/loginsight/cidata
   ```
   > Replace `<newnode>` with the IP address or FQDN of the new corresponding node in the new Cluster
1. Log into the new Clsuter nodes as root via SSH or Console.
1. Stop the loginsight service on the new Cluster on every node:
   ```
   service loginsight stop
   ```
1. On the new Cluster node, import the logs node by node:
   ```
   for bucket in $(ls /storage/core/loginsight/cidata/store | grep -v 'generation\|buckets\|strata_write.lock'); do echo y | /usr/lib/loginsight/application/sbin/bucket-index add $bucket --statuses archived; done
   ```
   > The Log Insight service **must** be stopped before running bucket-index.
   
   The `--statuses archived` implies the bucket is sealed and no further data will be added to it. To check all buckets have been added correctly:
   ```
   /usr/lib/loginsight/application/sbin/bucket-index show
   ```
1. On the new Cluster nodes, start the loginsight service:
   ```
   service loginsight start
   ```
You should now be able to access the historical data from the UI.