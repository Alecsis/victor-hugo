---
tags: [http, outil]
title: Curl - télécharger par HTTP
created: '2020-08-20T15:15:34.175Z'
modified: '2020-08-20T15:41:04.820Z'
---

# Curl - télécharger par HTTP

Curl permet de faire une requête vers un serveur HTTP, et il est possible de rediriger la réponse vers un fichier grâce au paramètre `-o`. Il s'utilise de la sorte :

```sh
curl <url> -o <sortie>
``` 

Par exemple :

```sh
curl https://alexis:8000/dossier/foo.txt -o /home/alexis/foo.txt
```
