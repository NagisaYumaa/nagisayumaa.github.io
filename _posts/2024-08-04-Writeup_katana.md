# Write-up katana

## 1. Enumeration

i used ```netdiscover```  to see real IP mylab

``` terminal
netdiscover
```
![alt text](/assets/img/katana/kat_netdiscover.png)

IP: ```192.168.100.18```

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.100.18
```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s

*   21 ```ftp``` 
*   22 ```ssh```
*   139-145 ```smb```
*   80-8715-7080-8088 ```http```

i used dirsearch to see dir maybe have more information .
![alt text](/assets/img/katana/kat_dirsearch.png)
## 2. Post Exploitation
i will upload shell and action on web port 8715

![alt text](/assets/img/katana/kat_www-data.png)

## 3. Privilage Escalation
i check ```capabilities``` and use ```gtfobin``` to get root

```
getcap / 2>/dev/null
```

![alt text](image.png)
