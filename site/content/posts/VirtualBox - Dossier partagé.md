---
tags: [config, virtualbox]
title: VirtualBox - Dossier partagé
created: '2020-08-13T13:14:03.945Z'
modified: '2020-08-18T19:49:32.600Z'
---

# VirtualBox - Dossier partagé

## Configurer virtualbox

Dans la barre de paramèters, chercher : `Périphériques > Dossiers Partagés > Réglages des dossiers partagés...`.

Ajouter un nouveau dossier partagé avec l'icône `+` en haut à droite de la fenêtre.

Cocher `montage automatique` et `configuration permanente`.

## Ajouter les droits à l'utilisateur

Le dossier partagé apartient à root par défaut. On peut ajouter le groupe `vboxsf` à l'utilisateur pour qu'il puisse tout de même y accéder :

```sh
$ sudo usermod -G vboxsf -a <user>
```
