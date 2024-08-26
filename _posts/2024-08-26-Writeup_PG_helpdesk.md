---
title: WRITE-UP PG_helpdesk
date: 2024-08-26 09:00:01 +0700
categories: [write up, proving ground, red team, oscp]
tags: [proving ground]     # TAG names should always be lowercase
---
# Write up PG_helpdesk

## Enumeration 
* enumaration nmap port IP ``` 192.168.228.43```

![alt text](/assets/img/PG_helpdesk/imgge.png)

135
139 - 445 smb
3389 RDP
8080 http

* check smb with nmap script

```
nmap --script smb-vuln*.nse -p 139,445 smb-vuln-scan --min-rate=2000 192.168.214.43 -Pn 
```
![alt text](/assets/img/PG_helpdesk/imgge-1.png)

* i payload with this command and get shell :>

![alt text](/assets/img/PG_helpdesk/imgge-3.png)

![alt text](/assets/img/PG_helpdesk/imgge-2.png)