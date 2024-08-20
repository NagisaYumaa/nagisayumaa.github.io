# Write-up  PG_assigment

* nmap to get port open

![alt text](image.png)

* on port 80 website is vuln ```IDOR``` to get cred ```gogs```

![alt text](image-1.png)

![alt text](image-2.png)

* gogs vuln RCE on create repos

![alt text](image-3.png)

* in user jane we find a script name /usr/bin/clean-tmp.sh this script make run  -exec with sudo
* and we must encrypt base64 and decyrpt to get shell

![alt text](image-4.png)