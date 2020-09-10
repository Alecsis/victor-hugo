---
tags: [cyber, outil]
title: Nmap - port scanner
created: '2020-08-11T13:54:17.306Z'
modified: '2020-08-18T19:49:02.380Z'
---

# Nmap - port scanner

| Paramètre | Description |
|-----------|-------------|
| -sV       | Attempts to determine the version of the services running |
| -p <x> or -p- | Port scan for port <x> or scan all ports |
| -Pn       | Disable host discovery and just scan for open ports |
| -A        | Enables OS and version detection, executes in-build scripts for further enumeration  |
| -sC       | Scan with the default nmap scripts |
| -v        | Verbose mode |
| -sU       | UDP port scan |
| -sS       | TCP SYN port scan |

Scanne tous les ports en mode SYN : `nmap -sS -p- <ip>`

Enumère les versions des services trouvés précédemment : `nmap -A -p<ports>`

Avec metasploit : `db_nmap <params> <ip>`
