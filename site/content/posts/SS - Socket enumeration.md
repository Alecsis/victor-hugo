---
tags: [cyber, outil]
title: SS - Socket enumeration
created: '2020-08-18T08:18:56.835Z'
modified: '2020-08-18T19:49:26.019Z'
---

# SS - Socket enumeration

`ss` est un outil qui permet d'énumérer les sockets qui fonctionnent sur un hôte.

| Paramètre | Description |
|-----------|-------------|
| -t       | Display TCP sockets |
| -u | Display UDP sockets |
| -l       | Displays only listening sockets |
| -p        | Shows the process using the socket  |
| -n       | Doesn't resolve service names |

La commande `ss -tulpn` montre toutes les connexions en cours.
