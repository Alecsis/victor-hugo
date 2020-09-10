---
tags: [linux, shell]
title: Linux - Shell avancé
created: '2020-08-21T06:32:14.404Z'
modified: '2020-08-21T06:51:20.526Z'
---

# Linux - Shell avancé

| Commande | Description |
|----------|-------------|
| cat | affiche le contenu d'un fichier |
| grep | filtrage par expressions régulières basiques |
| egrep | filtrage par expressions régulières étendues |
| tr | transposition ou supression de caractères dans une chaîne |
| pr | affiche un fichier avec n colonnes et m lignes par page |
| paste | concatène plusieurs fichier en un seul ou plusieurs lignes en une seule |
| head | affiche les premières lignes d'un fichier |
| tail | afficher les dernières lignes d'un fichier |
| cut | sélectionne sertains caractères dans une chaîne |
| sed | supprime ou remplace une partie d'une chaîne |
| awk | langage de script pour le traitement avancé de fichiers texte |
| sort | trie les lignes d'un fichier |
| uniq | supprime les doublons dans un fichier trié |
| nl | affiche un fichier en numérotant les lignes |
| split | découpe un fichier en plusieurs fichiers |
| date | affiche une date formatée |
| for | répète une commande plusieurs fois |
| seq | génère une liste de nombres |

## cut

`-d` : delimitateur
`-f<id>` : le champ à conserver, commence à 1

```sh
kali@kali:~$ cut -d: -f1 /etc/passwd | head
root
daemon
bin
sys
sync
games
man
lp
mail
news
```

## paste

Il faut le même nombre de lignes.

```sh
$ cat articles
routeur
switch
cable

$ cat prix
500€
100€
5€/m

$ paste articles prix 
routeur	500€
switch	100€
cable	5€/m
```

## join

Il faut une colonne commune pour faire la jointure.

```sh
$ cat articles 
100 routeur
200 switch
300 cable

$ cat prix 
100 500€
200 100€
300 4€/m

$ join articles prix
100 routeur 500€
200 switch 100€
300 cable 4€/m
```

## Boucle for

```sh
FILENAME=/etc/passwd
for user in $(cut -d: -f1 $FILENAME)
do
  echo $user
done
```

