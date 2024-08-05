---
title: WRITE-UP Drifting Blue 6
date: 2024-08-05 11:01:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-up Drifting Blue 6

## 1. Enumeration

i used ```netdiscover```  to see real IP mylab

``` terminal
netdiscover
```
![alt text](/assets/img/driftingblues6/db_netdiscover.png)

Ip : ``` 192.168.137.59```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.137.59
```
![alt text](/assets/img/driftingblues6/db_nmap.png)

only ```port 80```
![alt text](/assets/img/driftingblues6/db_dirsearch.png)
i using dirsearch and i get a zip file  is spammer

## 2. Post Exploitation

I using john to unzip file and i have a cred and i check web in url dirsearch i can login with this account 
![alt text](/assets/img/driftingblues6/db_getaccount.png)
![alt text](/assets/img/driftingblues6/db_80.png)

website have uploads file and i can takeover www-data by upload reverse shell
![alt text](/assets/img/driftingblues6/db_www_data.png)

## 3. Privilage Escalation

i check linux kernel OS using linux 3.2.0 can privilege with dirtycows
![alt text](/assets/img/driftingblues6/db_root.png)