---
title: WRITE-UP Pyexp
date: 2024-08-07 16:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---


#  Writeup pyexp (vie)
![alt text](/assets/img/pyexp/py_ip.png)

khi chay may co ip

![alt text](/assets/img/pyexp/py_nmap.png)

dung nmap thi thay 2 port ssh va mysql

![alt text](/assets/img/pyexp/py_crack_mysql.png)

brute mysql voi rockyou thi co dc account mysql co duoc 1 table cred

![alt text](/assets/img/pyexp/py_cred.png)

crack hash with table ta co dc cred co the ssh

dung sudo -l thi ta thay server chay code nhu tren 

raw_input read code tung dong vay chi can viet code tiep thi co the pass qua va len root

![alt text](/assets/img/pyexp/py_root.png)