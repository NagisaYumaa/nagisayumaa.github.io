---
title: WRITE-UP MoneyBox
date: 2024-08-05 00:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---
# Write-Up Money_box

## 1. Enumeration
i used netdicover to see where is my lab

```terminal
sudo netdiscover
```
![alt text](/assets/img/moneybox/mbox_netdiscover.png)

ip: ```192.168.100.20```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.20
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/moneybox/mbox_nmap.png)
* 21 ```ftp```
* 80 ```http```
* 22 ```ssh```

i check ```ftp``` with ```anonymous``` user i get file is ```trytofind.jpg```

```terminal
ftp 192.168.100.20
mget * 
yes
```
![alt text](/assets/img/moneybox/mbox_ftp.png)

i check ```http``` with ```dirsearch``` and i get text is ```3xtr4ctd4t4```
```
dirsearch -u http://192.168.100.20 -e 403,404
```
![alt text](/assets/img/moneybox/mbox_dirsearch.png)
## 2. Post Exploitation
i use ```steghide``` to get file ```data.txt``` and i used password i recon in web. So i know this site using weak password with user ```renu``` and i decided ssh bruteforce with ```hydra```

```terminal
stegide --extract -sf trytofind.jpg
hydra -l renu -P /home/kali/rockyou.txt ssh://192.168.100.20
```
![alt text](/assets/img/moneybox/mbox_message.png)

## 3. Privilage Escalation
after enum i see in user ```renu``` have ```id_rsa``` and i decided ssh with ```lily``` user with id and i can go user ```lily```
![alt text](/assets/img/moneybox/mbox_gotolily.png)

i'm using ```sudo -l ``` and check with ```gtfobin```  and i can get root :3 with perl 
![alt text](/assets/img/moneybox/mbox_root.png)