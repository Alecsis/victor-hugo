---
tags: [active directory, cyber, kerberos]
title: Active Directory - Kerberos
created: '2020-08-20T14:11:37.381Z'
modified: '2020-08-20T14:11:55.379Z'
---

# Active Directory - Kerberos

## Qu'est-ce que Kerberos ?

Kerberos est le service d'authentification par défaut pour les domaines Microsoft Windows. Il est destiné à être plus "sûr" que NTLM en utilisant une autorisation de ticket de tiers ainsi qu'un chiffrement plus fort. Même si NTLM dispose de beaucoup plus de vecteurs d'attaque, Kerberos présente encore quelques vulnérabilités sous-jacentes, tout comme NTLM, que nous pouvons utiliser à notre avantage.

## Terminologie commune 

- **Ticket d'attribution de ticket** (TGT) : un ticket d'attribution de ticket est un ticket d'authentification utilisé pour demander au TGS des tickets de service pour des ressources spécifiques du domaine.
- **Centre de distribution de clés** (KDC) : le centre de distribution de clés est un service d'émission de TGT et de tickets de service qui se compose du service d'authentification et du service d'octroi de tickets. **KDC = AS + TGS**.
- **Service d'authentification** (AS) : le service d'authentification émet des TGT à utiliser par le TGS dans le domaine pour demander l'accès à d'autres machines et tickets de service.
- **Ticket Granting Service** (TGS) : le Ticket Granting Service prend le TGT et retourne un ticket à une machine du domaine.
- **Service Principal Name** (SPN) : un Service Principal Name est un identifiant donné à une instance de service pour associer une instance de service à un compte de service du domaine. Windows exige que les services aient un compte de service de domaine, c'est pourquoi un service a besoin d'un ensemble de SPN.
- **Clé secrète à long terme KDC** (KDC LT Key) : la clé KDC est basée sur le compte de service KRBTGT. Elle est utilisée pour chiffrer le TGT et signer le PAC.
- **Clé secrète à long terme du client** (Client LT Key) : la clé du client est basée sur l'ordinateur ou le compte de service. Elle est utilisée pour vérifier l'horodatage chiffré et chiffrer la clé de session.
- **Service Long Term Secret Key** (Service LT Key) : la clé de service est basée sur le compte de service. Elle est utilisée pour chiffrer la partie service du ticket de service et signer le PAC.
- **Clé de session** : émise par le KDC lorsqu'un TGT est émis. L'utilisateur fournira la clé de session au KDC en même temps que le TGT lorsqu'il demande un ticket de service.
- **Certificat d'attribut de privilège** (PAC) : le PAC contient toutes les informations pertinentes de l'utilisateur, il est envoyé avec le TGT au KDC pour être signé par la clé LT cible et la clé LT du KDC afin de valider l'utilisateur.

## AS-REQ avec pré-authentification

L'étape AS-REQ de l'authentification Kerberos commence lorsqu'un utilisateur demande un TGT au KDC. Afin de valider l'utilisateur et de créer un TGT pour l'utilisateur, le KDC doit suivre ces étapes précises : 

- La première étape consiste pour l'utilisateur à chiffrer un hachage NT d'horodatage et à l'envoyer à l'AS. 
- Le KDC tente de déchiffrer l'horodatage de l'utilisateur à l'aide du hachage NT. 
- En cas de succès, le KDC émet un TGT ainsi qu'une clé de session pour l'utilisateur.

## Contenu des TGT

Afin de comprendre comment les titres de service sont créés et validés, nous devons commencer par la provenance des titres. Le TGT est fourni par l'utilisateur au KDC, en retour, le KDC valide le TGT et renvoie un titre de service.

## Contenu des tickets de service

Pour comprendre le fonctionnement de l'authentification Kerberos, nous devons d'abord comprendre le contenu de ces tickets et la manière dont ils sont validés. Un ticket de service contient deux parties :

- **Portion de service** : détails de l'utilisateur, clé de session, chiffre le ticket avec le hachage NTLM du compte de service.
- **Portion de l'utilisateur** : horodatage de validité, clé de session, chiffre avec la clé de session TGT.

## Aperçu de l'authentification Kerberos

1) **AS-REQ** : Le client demande un ticket d'authentification ou un ticket d'octroi de ticket (TGT).
2) **AS-REP** : Le centre de distribution de clés vérifie le client et renvoie un ticket d'authentification chiffré.
3) **TGS-REQ** : Le client envoie le TGT chiffré au serveur d'octroi de tickets (TGS) avec le nom du principal du service (SPN) auquel le client veut accéder.
4) **TGS-REP** : Le centre de distribution de clés (KDC) vérifie le TGT de l'utilisateur et que celui-ci a accès au service, puis envoie au client une clé de session valide pour le service.
5) **AP-REQ** : Le client demande le service et envoie la clé de session valide pour prouver que l'utilisateur a accès au service.
6) **AP-REP** : Le service accorde l'accès.

## Vue d'ensemble des tickets de Kerberos

Le principal ticket que vous verrez est un ticket d'entrée, qui peut se présenter sous différentes formes, comme un `.kirbi` pour Rubeus, un `.ccache` pour Impacket. Le ticket principal que vous verrez est un ticket `.kirbi`. Un ticket est généralement encodé en **base64** et peut être utilisé pour diverses attaques. Le TGT n'est utilisé qu'avec le KDC afin d'obtenir des tickets de service. Une fois que vous avez donné le TGT, le serveur obtient les détails de l'utilisateur, la clé de session, puis chiffre le ticket avec le hachage NTLM du compte de service. Votre TGT donne ensuite l'horodatage chiffré, la clé de session et le TGT chiffré. Le KDC authentifiera ensuite le TGT et rendra un ticket de service pour le service demandé. Un TGT normal ne fonctionnera qu'avec le compte de service qui lui est connecté, mais un KRBTGT vous permet d'obtenir n'importe quel ticket de service qui vous permet d'accéder à tout ce que vous voulez sur le domaine.

## Privilèges requis par type d'attaque

- **Enumération de Kerbrute** : aucun accès au domaine
- **Pass the Ticket** : accès en tant qu'utilisateur du domaine cible
- **Kerberoasting** : accès cutilisateur
- **AS-REP Roasting** : accès utilisateur
- **Ticket en or** : administrateur de domaine (AD pwned) 
- **Ticket d'argent** : hash de service requis 
- **Clé squelette** : administrateur de domaine (AD pwned)
