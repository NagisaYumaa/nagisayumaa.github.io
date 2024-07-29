---
title: WRITE-UP FUNBOX-7 / FUNBOX EASYENUM
date: 2024-07-30 00:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-Up Funbox-7 / funbox easyenum

## 1. Enumeration
i used netdicover to see where is my lab

![alt text](/assets/img/funbox7/funbox7_netdiscover.png)

ip : ``` 192.168.100.8 ```



i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.8
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/funbox7/funbox7_nmap.png)

i use dirsearch to find something
```
dirsearch -u http://192.168.100.8 -w /usr/share/dirb/wordlists/big.txt -e php -f php
```

* -e : output file php
* -f : force wordlist add.php in end file if file not end is php
![alt text](/assets/img/funbox7/funbox7_dirsearch.png)

after that i find ```mini.php``` help me to get shell to this lab

## 2. Post Exploitation

![alt text](/assets/img/funbox7/funbox7_shell.png)

i decided upload shell to take control ```www-data```

![alt text](/assets/img/funbox7/funbox7_www_data.png)

## 3. Privilegae Escalation

on dirsearch file we can recon ```phpmyadmin``` so i decide to see ```config``` file to see more in database . 

```
cat /etc/phpmyadmin/config-db.php
```
![alt text](/assets/img/funbox7/funbox7_account_phpmyadmin.png)

we can see pass is ``` tgbzhnujm! ``` with we focus on account is ```karla ```
so i decided ssh with that's account .
![alt text](/assets/img/funbox7/funbox7_ssh_karla.png)

now we can see account karla have group sudo :> so we can up to root with this user

```
sudo sudo /bin/bash
```

![alt text](/assets/img/funbox7/funbox7_root.png)

## GG o.o