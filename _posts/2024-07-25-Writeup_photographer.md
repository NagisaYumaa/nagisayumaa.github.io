---
title: WRITE-UP PHOTOGRAPHER
date: 2024-07-25 17:59:59 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-up Photographer

## 1. Enumeration

for ```vulnhub``` lab :3 i always use ```netdiscover``` first to see what is my IP i can resolve.

``` terminal
netdiscover
```

![alt text](/assets/img/photographer/photo_netdiscover.png)

``` 192.168.137.83 ```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.137.83
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

![alt text](/assets/img/photographer/photo_nmap.png)

port open is  ```80, 139, 445, 8000```

i use dir search to find something on website

![alt text](/assets/img/photographer/photo_dirsearch.png)

``` 192.168.137.83:8000 ``` it's koken :> it's EOF so i will focus on this .

so i was be stucked i can't see default credentials . So i think it's time to see another port :> 

hmmm 445-139 SMB ? mail ? maybe have some things

![alt text](/assets/img/photographer/photo_smbrecon.png)

i have email user maybe is 
``` daisa@photographer.com ``` and a letter ``` babygirl ```

## 2. Post Exploitation

okey let's become ```koken site``` and i will try with my information i have .

![alt text](/assets/img/photographer/photo_kokenlogin.png)

and it's correct i can login on this site with this credentials

![alt text](/assets/img/photographer/photo_kokenlogin2.png)

This site have a upload file but don't have prevent so i can upload a shell from to take over this site

 ![alt text](/assets/img/photographer/photo_upload.png) 

 ![alt text](/assets/img/photographer/photo_upload2.png)

 ![alt text](/assets/img/photographer/photo_upload3.png)

now we get userflag

![alt text](/assets/img/photographer/photo_userflag.png)

## 3. Privilage Escalation

i spawn reverse shell to make :v easy to look 

![alt text](/assets/img/photographer/photo_shell.png)

i used ``` sudo -l ``` to find some tricky i use ```gtfobin ``` and i see ```php7.2``` have SUID

![alt text](/assets/img/photographer/photo_SUID.png)

And It's time to root baby :V 

![alt text](/assets/img/photographer/photo_root.png)

## GG Special Thanks v1n1v131r4 For this lab :> and new knowleage :3