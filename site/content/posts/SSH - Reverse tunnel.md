---
tags: [outil, ssh]
title: SSH - Reverse tunnel
created: '2020-08-18T08:36:04.450Z'
modified: '2020-08-18T19:49:21.632Z'
---

# SSH - Reverse tunnel

La redirection de port SSH inverse spécifie que le port donné sur l'hôte du serveur distant doit être redirigé vers l'hôte et le port donnés du côté local.

- `-L` est un tunnel local (CLIENT --> VOUS). Si un site a été bloqué, vous pouvez rediriger le trafic vers un serveur que vous possédez et le consulter. Par exemple, si imgur a été bloqué au travail, vous pouvez faire `ssh -L 9000:imgur.com:80 user@example.com`. En allant sur `localhost:9000` sur votre machine, vous chargerez le trafic d'imgur en utilisant votre autre serveur.
- `-R` est un tunnel distant (VOUS --> CLIENT). Vous transférez votre trafic vers l'autre serveur pour que les autres puissent le consulter. Semblable à l'exemple ci-dessus, mais à l'inverse.
