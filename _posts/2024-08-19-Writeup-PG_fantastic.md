---
title: WRITE-UP fanatastic
date: 2024-08-19 09:00:01 +0700
categories: [write up, proving ground, red team, oscp]
tags: [proving ground]     # TAG names should always be lowercase
---

# Write-up PG_fanatastic

## 1. Enumeration

* nmap to see port and service open 

![alt text](/assets/img/PG_fanatastic/pg_fan_nmap.png)

* i'm check port web and service is grafana && promethus
* Grafana out of date so i can exploit ```CVE-2021-43798```

![alt text](/assets/img/PG_fanatastic/pg_fan_vuln.png)

## 2. Post Exploitation

https://github.com/jas502n/Grafana-CVE-2021-43798 i exploit and decrypt to get user and password ```sysadmin```

![alt text](/assets/img/PG_fanatastic/pg_fan_payload.png)

## 3. Privilage Escalation

* While access on ```sysadmin``` i check something can do with manual like ```id``` ```gtfobin```

![alt text](/assets/img/PG_fanatastic/pg_fan_sysadmin.png)

* i use ```linpeas.sh``` and i can see group disk in user ```sysadmin``` is misconfiguration and GG we get flag. 

![alt text](/assets/img/PG_fanatastic/pg_fan_priv.png)

![alt text](/assets/img/PG_fanatastic/pg_fan_root.png)