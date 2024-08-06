---
title: WRITE-UP Empire Breakout
date: 2024-08-06 16:00:01 +0700
categories: [write up, vulnhub, red team, oscp]
tags: [vulnhub]     # TAG names should always be lowercase
---

# Write-Up Empire Breakout
## 1. Enumeration

When Openlab we can see ip we can connect with lab

![alt text](/assets/img/empire_breakout/eb_ip.png)

This lab Open ```SMB``` service so we use ```enum4linux``` to check what happend insite
```
enum4linux 192.168.137.11
```

we get user is ```cyber```

![alt text](/assets/img/empire_breakout/eb_enumuser.png)

Check w/assets/img/empire_breakout/eb site we get a string . :> We decode and get a string ```Brain F ~~~```

![alt text](/assets/img/empire_breakout/eb_enumpass.png)

## 2. Post Exploitation

using cred we have we can login on site we can command shell and now we open the ```tunnel``` go to server 

![alt text](/assets/img/empire_breakout/eb_takeuser.png)

## 3. Privilage Escalation
in server we see ```tar``` action on root but we can use is normal because we have permition ```capability```

```
./tar -xvf backup.tar /var/backups/*.bak
./tar -cvf backup.tar
cat /var/backups/*.bak
```

![alt text](/assets/img/empire_breakout/eb_root.png)