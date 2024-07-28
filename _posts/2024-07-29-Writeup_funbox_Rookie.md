---
title: WRITE-UP FUNBOX ROOKIE
date: 2024-07-29 00:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-Up Funbox-2 / Funbox Rookie

## 1. Enumeration
i used netdicover to see where is my lab

```terminal
sudo netdiscover
```
ip is :> ```192.168.100.6```
![alt text](/assets/img/funbox2/funbox2_netdiscover.png)

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.6
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/funbox2/funbox2_nmap.png)

port open is 80 http , 22 ssh , 21 ftp 

![alt text](/assets/img/funbox2/funbox2_port21.png)

i check port 21 i can connect to get file with anonymous user and no password need

## 2. Post Exploitation

after get all file zip . I want unzip but require password so i'm using ``` johntheripper ``` to crack with ```rockyou``` wordlist and i can unzip ```tom ``` and ```cathrine``` 

```
zip2john catherine.zip > catherine.hashes
zip2jhon tom.zip > test.hashes
john catherine.hashes --wordlist=/home/kali/rockyou.txt
john test.hashes --wordlist=/home/kali/rockyou.txt
```

![alt text](/assets/img/funbox2/funbox2_crack_catherine.png)

![alt text](/assets/img/funbox2/funbox2_crack_tom.png)

after crack ```tom``` and ```catherine``` in file have id_rsa to connect ```ssh``` so i will connect them but only tom can use .

```
ssh -i id_rsa_tom tom@192.168.100.6
```
![alt text](/assets/img/funbox2/funbox2_login_ssh_tom.png)

## 3. Privilage Escalation

when i connect ```ssh ``` with ```tom``` account i use 

```
ls -la
```

![alt text](/assets/img/funbox2/funbox2_ls.png)

and they have two files ``` . sudo_as_admin_successfull ``` and ```.mysql_history ``` so i decided red file history maybe i can read something 
![alt text](/assets/img/funbox2/funbox2_root.png)

after that's i can use tom with ```sudo ``` with tom very easy to get root rightnow

```
sudo sudo /bin/sh
```

![alt text](/assets/img/funbox2/funbox2_password.png)

## GG and thank for knowleage .