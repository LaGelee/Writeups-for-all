# [Crack the hash](https://tryhackme.com/room/crackthehash)
### [TryHackMe](https://tryhackme.com)
Cracking hashes challenges

### Stats
Difficulty :   **Easy**

Sections   :   **2**

Questions  :   **9**

### 1.1) 48bb6e862e54f2a795ffc4e541caed4d
Using the website [CrackStation](https://crackstation.net/) we can easily determine the password.
CrackStation tells us that it is "easy" hashed in md5
#### `Answer : easy`


### 1.2) CBFDAC6008F9CAB4083784CBD1874F76618D2A97
Using the website [CrackStation](https://crackstation.net/) we can easily determine the password.
CrackStation tells us that it is "password123" hashed in sha1
#### `Answer : password123`


### 1.3) 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032
Using the website [CrackStation](https://crackstation.net/) we can easily determine the password.
CrackStation tells us that it is "letmein" hashed in sha256
#### `Answer : letmein`


### 1.4) $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom
At this point we need to use a brute forcing tool like HashCat. The hint says **"This type of hash can take a very long time to crack, so either filter rockyou for four character words, or use a mask for four lower case alphabetical characters."**. So first of all I will copy and filter the rockyou.txt wordlist.
```bash
$ cat /usr/share/wordlists/rockyou.txt | grep -E "^[a-z]{4}$" > rockyou_filter.txt
```
Then I am a able to crack it using HashCat. I put the hash in a hash.txt file. I use the following command where 3200 is the mode for this hash (bcrypt).
```bash
$ hashcat -m 3200 hash.txt rockyou_filter.txt
```
And after a few seconds:
``` bash
$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:bleh
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: bcrypt $2*$, Blowfish (Unix)
Hash.Target......: $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX...8wsRom
Time.Started.....: Sun Aug  8 20:57:05 2021 (38 secs)
Time.Estimated...: Sun Aug  8 20:57:43 2021 (0 secs)
Guess.Base.......: File (rockyou_filter.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       14 H/s (4.12ms) @ Accel:1 Loops:4 Thr:12 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 540/8541 (6.32%)
Rejected.........: 0/540 (0.00%)
Restore.Point....: 480/8541 (5.62%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4092-4096
Candidates.#1....: karl -> lord
Hardware.Mon.#1..: Temp: 50c Util:100% Core:1176MHz Mem:2505MHz Bus:8
```
The hash is cracked
#### `Answer : bleh`


### 1.5) 279412f945939ba78ce0758d3fd83daa
The hint says **"md4"**. So we know this is md4 and the id for it in hashcat is 900. So using a hash.txt file with the hash and the default rockyou.txt wordlist we can crack it.
```bash
$ hashcat -m 900 hash.txt /usr/share/wordlists/rockyou.txt
```
Ànd then...nothing... So I am using CrackStation and then....tada we have "Eternity22"
#### `Answer : Eternity22`




### 2.1) Hash: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85
To identify the hash we can use the software "hash-identifier" and the result of it is.
```bash
HASH: F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85

Possible Hashs:
[+] SHA-256
[+] Haval-256
```
I know the hashcat mode for it is 1400 so let's crack it.
```bash
$ hashcat -m 1400 hash.txt /usr/share/wordlists/rockyou.txt

[...]

f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85:paule
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: SHA2-256
Hash.Target......: f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f...2d0c85
Time.Started.....: Sun Aug  8 21:29:37 2021 (0 secs)
Time.Estimated...: Sun Aug  8 21:29:37 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........: 18263.8 kH/s (6.23ms) @ Accel:1024 Loops:1 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 327680/14344385 (2.28%)
Rejected.........: 0/327680 (0.00%)
Restore.Point....: 0/14344385 (0.00%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 123456 -> 01diva
Hardware.Mon.#1..: Temp: 42c Util:  0% Core:1097MHz Mem:2505MHz Bus:8
```
And we have the answer.
#### `Answer : paule`


### 2.2) Hash: 1DFECA0C002AE40B8619ECF94819CC1B
The hint says **"NTLM"** and the mode for it in hashcat is 1000 so :
```bash
$ hashcat -m 1000 hash.txt /usr/share/wordlists/rockyou.txt

[...]

1dfeca0c002ae40b8619ecf94819cc1b:n63umy8lkf4i    
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: NTLM
Hash.Target......: 1dfeca0c002ae40b8619ecf94819cc1b
Time.Started.....: Sun Aug  8 21:37:06 2021 (1 sec)
Time.Estimated...: Sun Aug  8 21:37:07 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  9049.3 kH/s (5.32ms) @ Accel:1024 Loops:1 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 5242880/14344385 (36.55%)
Rejected.........: 0/5242880 (0.00%)
Restore.Point....: 4915200/14344385 (34.27%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: omarsnork -> n1ckow3n
Hardware.Mon.#1..: Temp: 42c Util: 39% Core:1097MHz Mem:2505MHz Bus:8
```
We have our answer.
#### `Answer : n63umy8lkf4i`


### 2.3) Hash: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.     Salt: aReallyHardSalt
Looking on the internet we found that hashes with "$6$" are SHA512 hash. This is in hashcat mode 1800. Let's put the hash in the hash.txt file and crack it.
```bash
$ hashcat -m 1800 hash.txt /usr/share/wordlists/rockyou.txt

[...]

$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.:waka99
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: sha512crypt $6$, SHA512 (Unix)
Hash.Target......: $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPM...ZAs02.
Time.Started.....: Sun Aug  8 21:57:53 2021 (7 mins, 49 secs)
Time.Estimated...: Sun Aug  8 22:05:42 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:     6052 H/s (11.39ms) @ Accel:1 Loops:64 Thr:1024 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 2836480/14344385 (19.77%)
Rejected.........: 0/2836480 (0.00%)
Restore.Point....: 2831360/14344385 (19.74%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:4992-5000
Candidates.#1....: wakesake1 -> wade62
Hardware.Mon.#1..: Temp: 76c Util:100% Core:1176MHz Mem:2505MHz Bus:8
```
Like every time voila...

#### `Answer : waka99`

### 2.4) Hash: e5d8870e5bdd26602cab8dbe07a942c8669e56d6     Salt: tryhackme
Thanks to the hint we know that this hash is HMAC-SHA1. Now we need to find the mode for hashcat.
```bash
$ hashcat -h | grep HMAC-SHA1                              
    150 | HMAC-SHA1 (key = $pass)                          | Raw Hash, Authenticated
    160 | HMAC-SHA1 (key = $salt)                          | Raw Hash, Authenticated
```
We know that we have a salt so let's use mode 160. We need to put in the hash.txt file "hash:salt" so here "e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme". Now time to crack.
```bash
$ hashcat -m 160 hash.txt /usr/share/wordlists/rockyou.txt

[...]

e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme:481616481616
                                                 
Session..........: hashcat
Status...........: Cracked
Hash.Name........: HMAC-SHA1 (key = $salt)
Hash.Target......: e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme
Time.Started.....: Sun Aug  8 22:18:11 2021 (2 secs)
Time.Estimated...: Sun Aug  8 22:18:13 2021 (0 secs)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:  8167.2 kH/s (8.20ms) @ Accel:1024 Loops:1 Thr:64 Vec:1
Recovered........: 1/1 (100.00%) Digests
Progress.........: 12451840/14344385 (86.81%)
Rejected.........: 0/12451840 (0.00%)
Restore.Point....: 12124160/14344385 (84.52%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:0-1
Candidates.#1....: 581550 -> 4043235250
Hardware.Mon.#1..: Temp: 45c Util: 50% Core:1097MHz Mem:2505MHz Bus:8
```
:)

#### `Answer : 481616481616`

Write-up made wit :heart: by @LaGelee
