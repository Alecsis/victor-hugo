---
tags: [burpsuite, error, outil]
title: Burpsuite - Ajout du certificat
created: '2020-08-18T07:56:28.001Z'
modified: '2020-08-18T19:48:20.034Z'
---

# Burpsuite - Ajout du certificat

En activant le proxy, aller dans `127.0.0.1:8080`, cliquer sur `CA Certificate`, puis le sauvegarder.
Dans Firefox, aller dans `Préférences > Sécurité`. Tout en bas, `voir les certificats`.
Dans `Authorities`, sélectionner `import` et charger le certificat sauvegardé précédemmentet cocher la case `Trust this certificated to identify websites`.
