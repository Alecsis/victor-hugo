---
tags: [active directory, cyber, kerberos, outil]
title: Kerbrute - énumération d'AD
created: '2020-08-20T15:03:27.775Z'
modified: '2020-08-20T15:41:14.620Z'
---

# Kerbrute - énumération d'AD

Kerbrute est un outil d'énumération utilisé pour brute-forcer et énumérer les utilisateurs de l'AD en abusant de la pré-authentification Kerberos.

## Aperçu des abus de pré-authentification

En brute-forçant la pré-authentification Kerberos, vous ne déclenchez pas l'événement `le compte a échoué à se connecter` qui peut lancer des alertes aux Blue teams. Lorsque vous utilisez le brute-force par Kerberos, vous pouvez le faire en n'envoyant qu'une seule trame UDP au KDC, ce qui vous permet d'énumérer les utilisateurs du domaine à partir d'une liste de mots.

## Installation

1) Télécharger le binaire précompilé pour son OS : https://github.com/ropnop/kerbrute/releases
2) Renommer `kerbrute_linux_amd64` en `kerbrute` : `mv kerbrute_linux_amd64 kerbrute`
3) Rendre kerbrute exécutable :`chmod +x kerbrute`

## Enumération des utilisateurs avec Kerbrute

Enumérer les utilisateurs permet de savoir quels comptes d'utilisateurs se trouvent sur le domaine cible et quels comptes pourraient potentiellement être utilisés pour accéder au réseau.

1) `cd` dans le répertoire de Kerbrute
2) Y télécharger une liste de mots : https://github.com/Cryilllic/Active-Directory-Wordlists/blob/master/User.txt

```sh
curl https://raw.githubusercontent.com/Cryilllic/Active-Directory-Wordlists/master/User.txt -o user.txt
```

3) `./kerbrute userenum --dc CONTROLLER.local -d CONTROLLER.local <liste de mots>` 
