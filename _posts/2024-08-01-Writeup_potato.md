---
title: WRITE-UP Potato
date: 2024-08-01 00:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-Up Potato

## 1. Enumeration
i used netdicover to see where is my lab
![alt text](/assets/img/potato/ptt_netdiscover.png)

ip : ``` 192.168.100.12 ```

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.8
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s
  
![alt text](/assets/img/potato/ptt_nmap.png)

* 2112 is ```ftp``` can login with anonymous account
* 80 is ```http```
* 22 is ```ssh```

get file with ftp

```
ftp 192.168.100.12 2112
mget *
```

![alt text](/assets/img/potato/ptt_ftp.png)

let's see file ```index.php.bak```

![alt text](/assets/img/potato/ptt_backup_index.png)

## 2. Post Exploitation

it's a form login with vuln is ```strcmp```  Binary safe string comparison

if admin post username is ```admin``` and password same your password you set you can go with account .So if we compare with a[] with something ? answer is ```zero``` rightnows we can login with admin account and this web use this backup in ```/admin/index.php``` let's go

![alt text](/assets/img/potato/ptt_login.png)

after login, i saw this site can read logs .But it's vuln ``` path-travelsal ``` i can read file in server

![alt text](/assets/img/potato/ptt_path_travelsal.png)

when i read file ``` /etc/passwd ``` i can decode password of user ```webservice``` by using ```johntheripper```

```
john webserver.hashes
```

after crack i can get pass ```webadmin``` is ```dragon```

![alt text](/assets/img/potato/ptt_ssh.png)

## 3. Privilage Escalation

i'm using ``` sudo -l ``` to want to see i can run sudo to get ```root``` user and i get everything can run after path ```/notes/``` with ```sudo``` and ```nice``` command

And we can borrow ```path travelsal ``` file in linux to go us to ```/bin/bash``` to get ```root```

```
sudo -l

sudo nice /notes/../bin/bash
```

![alt text](/assets/img/potato/ptt_root.png)
## GG and special thanks for new knowleaged