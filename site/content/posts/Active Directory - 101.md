---
tags: [active directory, cloud AD, cyber, security, tryhackme, windows]
title: Active Directory - 101
created: '2020-08-20T11:43:49.177Z'
modified: '2020-08-20T14:12:11.291Z'
---

# Active Directory - 101

Active Directory est le service d'annuaire pour les réseaux de domaines Windows. Il est utilisé par un grand nombre des plus grandes entreprises actuelles et constitue une compétence essentielle à comprendre lorsqu'on attaque Windows.

## Introduction

### Qu'est-ce qu'Active Directory ?

Active Directory est un ensemble de machines et de serveurs connectés à l'intérieur de domaines, qui font collectivement partie d'une plus grande forêt de domaines, qui constituent le réseau Active Directory. Active Directory contient de nombreux éléments fonctionnels, dont la plupart seront couverts dans les tâches à venir. Pour en savoir plus, consultez la liste des composants d'Active Directory et familiarisez-vous avec les différents éléments d'Active Directory : 

- Contrôleurs de domaine
- Forêts, arbres, domaines
- Utilisateurs + Groupes 
- Approbations (approbations)
- Stratégies (policies)
- Services de domaine

Toutes ces parties d'Active Directory se réunissent pour former un grand réseau de machines et de serveurs. Maintenant que nous savons ce qu'est Active Directory, parlons du pourquoi ?

### Pourquoi utiliser Active Directory ?

La majorité des grandes entreprises utilisent Active Directory parce qu'il permet de contrôler et de surveiller les ordinateurs de leurs utilisateurs grâce à un contrôleur de domaine unique. Il permet à un seul utilisateur de se connecter à n'importe quel ordinateur du réseau Active Directory et d'avoir accès à ses fichiers et dossiers stockés sur le serveur, ainsi qu'au stockage local sur cette machine. Cela permet à n'importe quel utilisateur de l'entreprise d'utiliser n'importe quelle machine que l'entreprise possède, sans avoir à installer plusieurs utilisateurs sur une même machine. Active Directory fait tout cela pour vous.

Maintenant que nous connaissons le quoi et le pourquoi d'Active Directory, passons à son fonctionnement et à ses fonctions. 

## Active Directory Physique

L'Active Directory physique est constitué des serveurs et des machines sur place, qui peuvent être des contrôleurs de domaine, des serveurs de stockage ou des machines d'utilisateurs de domaine ; tout ce qui est nécessaire à un environnement Active Directory en dehors du logiciel.

### Contrôleurs de domaine

Un contrôleur de domaine est un **Windows Server** sur lequel sont installés des **Active Directory Domain Services** (AD DS) et qui a été promu au rang de contrôleur de domaine dans la forêt. Les contrôleurs de domaine sont le centre d'Active Directory :

- Ils détiennent les AD DS data stores 
- Gèrent les services d'authentification et d'autorisation 
- Reproduisent les mises à jour des autres contrôleurs de domaine dans la forêt
- Permettent à l'administrateur d'accéder à la gestion des ressources du domaine

### AD DS Data Store

L'Active Directory Data Store contient les bases de données et les processus nécessaires pour stocker et gérer les informations de l'annuaire telles que les utilisateurs, les groupes et les services. Les caractéristiques de l'AD DS Data store sont :

- Contenir le **NTDS.dit** : une base de données qui contient toutes les informations d'un contrôleur de domaine Active Directory ainsi que les hachages de mots de passe pour les utilisateurs du domaine
- l'AD DS DS est stocké par défaut dans `%SystemRoot%\NTDS` et accessible uniquement par le contrôleur de domaine.

## La forêt

La forêt est ce qui définit tout ; c'est le conteneur qui contient tous les autres éléments du réseau. Sans la forêt, tous les autres arbres et domaines ne pourraient pas interagir.

### Vue d'ensemble des forêts

Une forêt est une collection d'un ou plusieurs arbres de domaine à l'intérieur d'un réseau Active Directory. C'est ce qui permet de classer les parties du réseau dans leur ensemble.

La forêt est constituée de :

- **Arbres** : Une hiérarchie de domaines dans les services de domaine Active Directory
- **Domaines** : Utilisés pour regrouper et gérer des objets 
- **Unités organisationnelles** (OU) : Conteneurs pour les groupes, les ordinateurs, les utilisateurs, les imprimantes et autres OU
- **Approbations** (approbations) : Permet aux utilisateurs d'accéder à des ressources dans d'autres domaines
- **Objets** : utilisateurs, groupes, imprimantes, ordinateurs, partages
- **Services de domaine** : Serveur DNS, LLMNR, IPv6
- **Schéma de domaine** : Règles pour la création d'objets

## Utilisateurs + Groupes

Les utilisateurs et les groupes qui se trouvent dans un Active Directory dépendent de vous ; lorsque vous créez un contrôleur de domaine, il est livré avec des groupes par défaut et deux utilisateurs par défaut : **Administrateur** et **invité**. C'est à vous de créer de nouveaux utilisateurs et de créer de nouveaux groupes pour ajouter des utilisateurs.

### Aperçu des utilisateurs

Les utilisateurs sont au cœur d'Active Directory. Il existe quatre principaux types d'utilisateurs que vous trouverez dans un réseau Active Directory. cependant, il peut y en avoir davantage selon la façon dont une entreprise gère les autorisations de ses utilisateurs. Les quatre types d'utilisateurs sont les suivants : 

- **Les administrateurs de domaine** : les grands patrons. ils contrôlent les domaines et sont les seuls à avoir accès au contrôleur de domaine.
- **Comptes de service** (peuvent être des administrateurs de domaine) : Ils ne sont pour la plupart jamais utilisés, sauf pour la maintenance des services, ils sont requis par Windows pour des services tels que SQL pour coupler un service avec un compte de service
- **Administrateurs locaux** : Ces utilisateurs peuvent apporter des modifications aux machines locales en tant qu'administrateur et peuvent même contrôler d'autres utilisateurs normaux, mais ils ne peuvent pas accéder au contrôleur de domaine
- **Utilisateurs du domaine** : Ce sont vos utilisateurs quotidiens. Ils peuvent se connecter sur les machines auxquelles ils ont l'autorisation d'accéder et peuvent avoir des droits d'administrateur local sur les machines selon l'organisation.

### Aperçu des groupes

Les groupes facilitent l'attribution d'autorisations aux utilisateurs et aux objets en les organisant en groupes avec des autorisations spécifiques. Il existe deux grands types de groupes Active Directory : 

- **Groupes de sécurité** : ces groupes sont utilisés pour spécifier les autorisations pour un grand nombre d'utilisateurs
- **Groupes de distribution** : ces groupes sont utilisés pour spécifier les listes de distribution de courrier électronique. En tant qu'attaquant, ces groupes nous sont moins utiles, mais ils peuvent toujours être utiles dans le cadre d'un dénombrement

### Groupes de sécurité par défaut 

Il y a beaucoup de groupes de sécurité par défaut. Voici un bref aperçu :

- **Contrôleurs de domaine** : Tous les contrôleurs de domaine dans le domaine
- **Domain Guests** : Tous les invités du domaine
- **Utilisateurs du domaine** : Tous les utilisateurs du domaine
- **Ordinateurs de domaine** : Tous les postes de travail et serveurs reliés au domaine
- **Administrateurs de domaine** : Administrateurs désignés du domaine
- **Enterprise Admins** : Administrateurs désignés de l'entreprise
- **Schema Admins** : Administrateurs désignés du schéma
- **Administrateurs DNS** : Groupe des administrateurs DNS
- **Proxy de mise à jour DNS** : Clients DNS qui sont autorisés à effectuer des mises à jour dynamiques pour le compte de certains autres clients (tels que les serveurs DHCP).
- **Groupe autorisé de réplication des mots de passe RODC** : Les membres de ce groupe peuvent faire répliquer leurs mots de passe à tous les contrôleurs de domaine en lecture seule du domaine
- **Group Policy Creator Owners** : Les membres de ce groupe peuvent modifier la politique de groupe pour le domaine
- **Groupe de réplication des mots de passe RODC refusés** : Les membres de ce groupe ne peuvent pas faire répliquer leurs mots de passe à aucun contrôleur de domaine en lecture seule dans le domaine
- **Utilisateurs protégés** : Les membres de ce groupe bénéficient de protections supplémentaires contre les menaces de sécurité liées à l'authentification. Voir http://go.microsoft.com/fwlink/?LinkId=298939 pour plus d'informations.
- **Éditeurs de certificats** : Les membres de ce groupe sont autorisés à publier des certificats dans le répertoire
- **Contrôleurs de domaine en lecture seule** : Les membres de ce groupe sont des contrôleurs de domaine en lecture seule dans le domaine
- **Contrôleurs de domaine en lecture seule dans l'entreprise** : Les membres de ce groupe sont des contrôleurs de domaine en lecture seule dans l'entreprise
- **Key Admins** : Les membres de ce groupe peuvent effectuer des actions administratives sur les objets clés du domaine.
- **Enterprise Key Admins** : Les membres de ce groupe peuvent effectuer des actions administratives sur les objets clés de la forêt.
- **Contrôleurs de domaine clonables** : Les membres de ce groupe qui sont des contrôleurs de domaine peuvent être clonés.
- **Serveurs RAS et IAS** : Les serveurs de ce groupe peuvent accéder aux propriétés d'accès à distance des utilisateurs

## Approbations + stratégies

Les approbations et stratégies vont de pair pour aider le domaine et les arbres à communiquer entre eux et maintenir la "sécurité" à l'intérieur du réseau. Ils mettent en place les règles qui régissent la manière dont les domaines à l'intérieur d'une forêt peuvent interagir entre eux, la manière dont une forêt extérieure peut interagir avec la forêt, et les règles ou stratégies générales qu'un domaine doit suivre.

### Vue d'ensemble des approbations de domaine

Les approbations sont un mécanisme mis en place pour que les utilisateurs du réseau puissent accéder à d'autres ressources du domaine. Dans la plupart des cas, les approbations décrivent la manière dont les domaines à l'intérieur d'une forêt communiquent entre eux. Dans certains environnements, les approbations peuvent être étendus à des domaines externes et même à des forêts dans certains cas.
Il existe deux types de approbations qui déterminent la manière dont les domaines communiquent : 

- **Directionnel** : la direction de la confiance passe d'un domaine de confiance à un domaine de confiance.
- **Transitive** : la relation de confiance s'étend au-delà de deux domaines seulement pour inclure d'autres domaines de confiance.

Le type d'approbations mis en place détermine la manière dont les domaines et les arbres d'une forêt sont capables de communiquer et d'envoyer des données entre eux lorsqu'ils attaquent un environnement Active Directory. On peut parfois abuser de ces approbations pour se déplacer latéralement dans le réseau. 

### Aperçu des stratégies de domaine

Les stratégies constituent une très grande partie d'Active Directory, elles dictent la manière dont le serveur fonctionne et les règles qu'il suivra ou non. On peut considérer les stratégies de domaine comme des groupes de domaines, sauf qu'au lieu de permissions, elles contiennent des règles, et qu'au lieu de s'appliquer uniquement à un groupe d'utilisateurs, les stratégies s'appliquent à un domaine dans son ensemble. Elles font simplement office de livre de règles pour Active Directory qu'un administrateur de domaine peut modifier et altérer comme il le juge nécessaire pour assurer le bon fonctionnement et la sécurité du réseau.

En plus de la très longue liste de stratégies de domaine par défaut, les administrateurs de domaine peuvent choisir d'ajouter leurs propres stratégies qui ne sont pas déjà sur le contrôleur de domaine, par exemple : si vous souhaitez désactiver Windows Defender sur toutes les machines du domaine, on peut créer un nouvel objet de stratégie de groupe pour désactiver Windows Defender. 

Les options de stratégies de domaine sont presque infinies et constituent un facteur important pour les attaquants lorsqu'ils recensent un réseau Active Directory. Quelques-unes des nombreuses stratégies qui sont proposées par défaut ou que on peut créer dans un environnement Active Directory : 

- **Désactiver le Windows Defender** : désactive le Windows Defender sur chaque machine du domaine
- **Communication par signature numérique** (toujours) : peut désactiver ou activer la signature SMB sur le contrôleur de domaine

## Active Directory Domain Services + Authentification

Les services de domaine Active Directory sont les fonctions essentielles d'un réseau Active Directory. Ils permettent la gestion du domaine, des certificats de sécurité, des LDAP, et bien plus encore. C'est ainsi que le contrôleur de domaine décide de ce qu'il veut faire et des services qu'il veut fournir pour le domaine.

### Aperçu des services de domaine

Les services de domaine sont exactement ce dont ils ont l'air. Ce sont des services que le contrôleur de domaine fournit au reste du domaine ou de l'arbre. Il existe un large éventail de services divers qui peuvent être ajoutés à un contrôleur de domaine. Cependant, nous ne passerons en revue que les services par défaut qui sont fournis lorsque l'on configurera un serveur Windows comme contrôleur de domaine. Les services de domaine par défaut : 

- **LDAP** : Lightweight Directory Access Protocol ; assure la communication entre les applications et les services d'annuaire
- **Services de certification** : permet au contrôleur de domaine de créer, de valider et de révoquer les certificats de clé publique
- **DNS, LLMNR, NBT-NS** : Services de noms de domaine pour l'identification des noms d'hôtes IP

### Aperçu de l'authentification des domaines

La partie la plus importante d'Active Directory, **ainsi que la partie la plus vulnérable** d'Active Directory, est constituée par les protocoles d'authentification mis en place. Il existe deux principaux types d'authentification pour Active Directory : **NTLM** et **Kerberos**. 

  - **Kerberos** : le service d'authentification par défaut pour Active Directory utilise des tickets d'attribution de tickets et des tickets de service pour authentifier les utilisateurs et leur donner accès à d'autres ressources dans le domaine.
  - **NTLM**  le protocole d'authentification par défaut de Windows utilise un protocole crypté de défi/réponse

Les services du domaine Active Directory sont le **principal point d'accès pour les attaquants** et contiennent certains des protocoles les plus vulnérables pour Active Directory.

## AD dans le cloud

Récemment, un changement dans Active Directory a poussé les entreprises à mettre en place des réseaux sur le cloud pour leurs entreprises. Le fournisseur de services cloud le plus connu est Azure AD. Ses paramètres par défaut sont beaucoup plus sûrs qu'un réseau Active Directory physique sur site ; cependant, le cloud AD peut encore présenter des vulnérabilités. 

### Vue d'ensemble de l'AD azure

Azure joue le rôle d'intermédiaire entre l'Active Directory physique et l'ouverture de session des utilisateurs. Cela permet une transaction plus sûre entre les domaines, ce qui rend inefficace un grand nombre d'attaques Active Directory.

### Vue d'ensemble de la sécurité dans le nuage

La meilleure façon de démontrer comment le cloud prend des précautions de sécurité au-delà de ce qui est déjà fourni avec un réseau physique est de montrer une comparaison avec un environnement Active Directory cloud : 

| Serveur Windows AD |  Azure AD |
|--------------------|-----------|
| LDAP | API REST |
| NTLM | OAuth/SAML |
| Kerberos | OpenID |
| OU Trees | structure plate |
| Domaines & forêts | locataire (tentants) |
| Approbations | Invités |
