---
tags: [outil, ssh]
title: SSH - Génération d'une nouvelle clé
created: '2020-08-17T12:26:50.684Z'
modified: '2020-08-18T19:48:46.648Z'
---

# SSH - Génération d'une nouvelle clé

Créons la clé dans un premier temps :

```
kali@kali:~$ ssh-keygen -t rsa -b 4096 -C "adresse@mail.com"
```

Puis ajoutons la clé au `ssh-agent` :

```sh
kali@kali:~$ ssh-add ~/.ssh/id_rsa
Identity added: /home/kali/.ssh/id_rsa (adresse@mail.com)
```

Ajout sur github : compte > paramètres > clés SSH & GPG 

```sh
kali@kali:~$ sudo apt-get install xclip
kali@kali:~$ xclip -sel clip < ~/.ssh/github_id_rsa.pub
```
