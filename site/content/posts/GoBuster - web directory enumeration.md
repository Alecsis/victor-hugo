---
tags: [cyber, outil]
title: GoBuster - web directory enumeration
created: '2020-08-11T14:37:47.410Z'
modified: '2020-08-18T19:48:56.332Z'
---

# GoBuster - web directory enumeration

| Paramètre | Description |
|-----------|-------------|
| -e | Print the full URLs in your console |
| -u | The target URL |
| -w | Path to your wordlist |
| -U and -P | Username and Password for Basic Auth |
| -p <x> | Proxy to use for requests |
| -c <http cookies> | Specify a cookie for simulating your auth |

Exemple : `gobuster dir -u http://<ip>:3333 -w <word list location>`
