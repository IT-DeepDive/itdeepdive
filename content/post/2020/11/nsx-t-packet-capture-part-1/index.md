---
title: "NSX-T Packet Capture Part 1: East - West Traffic on Overlay Segment"
date: 2020-11-30
description: "NSX-T Packet Capture Part 1: East - West Traffic on Overlay Segment using nsxcli and pktcap-uw."
featured: true
toc: true
codeMaxLines: 10
codeLineNumbers: false
categories:
- Troubleshooting
series:
- NSX-T
tags:
- packet capture
- nsxcli
- vmware
- networking
- nsx-t

---

In this article, we are going to perform packet captures at different capture points between 2 VMs running on 2 Overlay segments attached to a T1 Router.

## Topology
The Tier-1 Gateway does not provide stateful services so it's not connected to an Edge Cluster. It worth mentioning it because the data path will only go from ESXi host to ESXi host without the need of traversing the physical network.

![Topology](img/20201130-115615.png)

Let's to a Traceflow and see what the data path looks like:
![Data path](img/20201130-115806.png)

The traffic goes from the VM vnic down to the vdr port 

We need to get the PortIDs for:
- T1-Web-01
- T1-DB-01
- vdr port on the source host (Where the routing decisions take place)

We can use many different methods to find this information, but i thhink the most efficient is nsxdb-cli
1. Get an SSH session to the host where the source VM (T1-Web-01) is running and do: `nsxdp-cli vswitch instance list`
   ![vswitch instance list on ESXi4](img/20201130-133002.png)
1. Now do the same on the second host:
   ![vswitch instance list on ESXi5](img/20201130-133239.png)

We are interested in:

Client | PortID | MAC | Uplink
-|-|-|-
T1-Web-01.eth0 | 100663322 | 00:50:56:92:49:09 | vmnic2
vdr-vdrPort (on source host) | 100663318 | 02:50:56:56:44:52 | vmnic2
T1-DB-01.eth0 | 134217801 | 00:50:56:92:98:b6 | vmnic2

Ok, now let's start a continuous ping from T1-Web-01 to T1-DB-01: ![Continuous ping](img/20201130-133850.png)

And start the capture at the switchport level:
```
# pktcap-uw --switchport 100663322 --dir 2 -o - | tcpdump-uw -ner -
```
![Capture point 1](img/20201130-134437.png)

The packet is sent from the VM vnic `00:50:56:92:49:09` to the vdrport `02:50:56:56:44:52`. ICMP replies comes from the same MAC.

On the other side `T1-DB-01` the very same thing is happening. ICMP packets coming from `02:50:56:56:44:52` are delivered to `00:50:56:92:98:b6` which replies to the same:
![Capture at the destination](img/20201130-141313.png)

Now let's dig a little bit deeper and follow the data path and capture at different capture points:
![Capture Porints](img/20201130-160404.png)

## Capturing traffic using nsxcli
From [NSX-T Command-Line Interface Reference](https://vdc-download.vmware.com/vmwb-repository/dcr-public/cc42e3c1-eb34-4567-a916-147e79798957/8264605c-a5e1-49a8-b603-cc78621eeeab/cli.html)

> **Sintax:** [`start capture interface <interface-name> [direction <direction>] [file <filename>] [count <packet-count>] [expression <expression>]`](https://vdc-download.vmware.com/vmwb-repository/dcr-public/cc42e3c1-eb34-4567-a916-147e79798957/8264605c-a5e1-49a8-b603-cc78621eeeab/cli.html#start%20capture%20interface%20%3Cinterface-name%3E%20[direction%20%3Cdirection%3E]%20[file%20%3Cfilename%3E]%20[count%20%3Cpacket-count%3E]%20[expression%20%3Cexpression%3E])

Point | Description | Capture with nsxcli 
:-:|-|-
1 | VM vnic switchport | `nsxcli -c start capture interface 100663322 direction dual expression ipproto 0x01`
2 | Before the DFW | `nsxcli -c start capture dvfilter nic-534759-eth0-vmware-sfw.2 stage pre expression ipproto 0x01`
3 | After the DFW | `nsxcli -c start capture dvfilter nic-534759-eth0-vmware-sfw.2 stage post expression ipproto 0x01`
4 | At the vdr Port | `nsxcli -c start capture interface vdrPort expression ipproto 0x01`<br>Source host (requests only):![](img/20201130-172634.png) Destination host (replies only):![](img/20201130-172741.png)
5 | N-DVS Uplink | `nsxcli -c start capture interface c9b5052b-9e46-4c9e-9b2a-8b1b46e26796 direction dual expression ipproto 0x01`<br>To get the DVPortID:![](img/20201130-183500.png)
6 | Physical Uplink | `nsxcli -c start capture interface vmnic2 direction dual`<br>Here the traffic is encapsulated into GENEVE protocol ![](img/20201130-195024.png)
7 | Before the DFW | To get the dvfilter name: ![](img/20201130-195506.png)<br>`nsxcli -c start capture dvfilter nic-534759-eth0-vmware-sfw.2 stage pre expression ipproto 0x01`
8 | After the DFW | `nsxcli -c start capture dvfilter nic-7925274-eth0-vmware-sfw.2 stage post expression ipproto 0x01`
:-:|-|-
9 | VM vnic switchport | `nsxcli -c start capture interface 134217801 direction dual expression ipproto 0x01`

## Capturing traffic using pktcap-uw

Point | Description | Capture with pktcap-uw
:-:|-|-
1 | VM vnic switchport | `pktcap-uw --switchport 100663322 --dir 2 --dstip 172.16.102.11 -o - | tcpdump-uw -ner -`
2 | Before the DFW | `pktcap-uw --switchport 100663322 --dir 2 --stage 0 --dstip 172.16.102.11 -o - | tcpdump-uw -ner -`
3 | After the DFW | `pktcap-uw --switchport 100663322 --dir 2 --stage 1 --dstip 172.16.102.11 -o - | tcpdump-uw -ner -`
4 | At the vdr Port | `pktcap-uw --switchport 100663318 --dir 2 --dstip 172.16.102.11 -o - | tcpdump-uw -ner -`
5 | N-DVS Uplink | `pktcap-uw --switchport 2248146957 --dir 2 --ip 172.16.103.11 -o - | tcpdump-uw -ner -`
6 | Physical Uplink | Fitering the destination VTEP IP and IP<br>`pktcap-uw --uplink vmnic2 --capture UplinkSndKernel --dstip 192.168.11.151 -o - | tcpdump-uw -ner - | grep 172.16.10`<br>`pktcap-uw --uplink vmnic2 --capture UplinkRcvKernel --srcip 192.168.11.151 -o - | tcpdump-uw -ner - | grep 172.16.10`
