---
title: WRITE-UP assignment
date: 2024-08-20 09:00:01 +0700
categories: [write up, proving ground, red team, oscp]
tags: [proving ground]     # TAG names should always be lowercase
---
# Write-up  PG_assigment

* nmap to get port open

![alt text](/assets/img/PG_assignment/image.png)

* on port 80 website is vuln ```IDOR``` to get cred ```gogs```

![alt text](/assets/img/PG_assignment/image-1.png)

![alt text](/assets/img/PG_assignment/image-2.png)

* gogs vuln RCE on create repos

![alt text](/assets/img/PG_assignment/image-3.png)

* in user jane we find a script name /usr/bin/clean-tmp.sh this script make run  -exec with sudo
* and we must encrypt base64 and decyrpt to get shell

```
 echo " 'revese-shell-encrypt-base64' | base64 -d | bash "
```

![alt text](/assets/img/PG_assignment/image-4.png)