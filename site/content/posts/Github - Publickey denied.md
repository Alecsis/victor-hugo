---
tags: [config, error, github]
title: Github - Publickey denied
created: '2020-08-13T13:09:54.242Z'
modified: '2020-08-18T19:48:51.960Z'
---

# Github - Publickey denied

Cas d'utilisation :

```sh
$ git clone git@github.com:<user>/<repo>.git
```

Erreur :

```
Cloning into '<repo>'...
Permission denied (publickey).
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
```

Solution : créer le fichier `~/.ssh/config` et y ajouter les lignes suivantes :

```
Host github.com
User git
Hostname github.com
PreferredAuthentications publickey
Port 22
IdentityFile <chemin vers la clé privée>
```
