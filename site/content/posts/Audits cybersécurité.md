---
tags: [audits]
title: Audits cybersécurité
created: '2020-08-14T14:57:20.953Z'
modified: '2020-08-17T15:50:30.658Z'
---

# Audits cybersécurité

## Scan de vulnérabilité

Un scan de vulnérabilité est un examen systématique des faiblesses de sécurité d'un système d'information à l’aide d’outils de scanning automatisés. Cela évalue si le système est susceptible de présenter des vulnérabilités connues, attribue des niveaux de gravité à ces vulnérabilités et recommande des mesures correctives ou d'atténuation, si et quand cela est nécessaire. A la différence d'un test d'intrusion, l'audit de vulnérabilité ne simule pas un attaquant motivé et qui y investit un temps conséquent. Ainsi, ce scan est un préalable requis avant un test d’intrusion car il met en évidence des vulnérabilités dont la recherche ne nécessite pas l’expertise technique d’un auditeur.

## Test d'intrusion (pentest)

Le test d'intrusion est une opération qui consiste à tester un système informatique afin de déceler des vulnérabilités. Pour ce faire, le tiers en charge du test va adopter une démarche d'attaquant malveillant similaire à celle d'un attaquant réel. Ce type d’audit vise deux objectifs : évaluer le risque de piratage, et identifier des pistes pour réduire ce risque.

### Les différentes approches d’un test d’intrusion

Il existe 3 scénarios d’attaque permettant de découvrir des vulnérabilités à tous les niveaux de privilèges :

-	Boîte noire : les auditeurs n'ont aucune information sur la cible. Ce scénario simule un attaquant externe qui ne connaîtrait pas le système. 
-	Boîte grise : les auditeurs utilisent un simple accès utilisateur avec de droits restreints utilise pour les tests. Ce scénario simule un attaquant ayant un accès interne à l’entreprise : un collaborateur ayant accès à des informations, un hacker qui aurait réussi à avoir accès à un compte utilisateur au sein de l’organisation. 
-	Boîte blanche : les auditeurs ont un accès à toute la documentation fonctionnelle et technique nécessaire pour les tests. Ce scénario simule un attaquant interne à l’organisation : un développeur, un administrateur réseau…

Il existe également 3 types d’accès au système audité :

-	Sur site : les auditeurs se rendent sur les lieux de l’organisation pour effectuer les tests.
-	Externe, avec VPN (réseau privé virtuel) : les auditeurs effectuent les tests à distance, en ayant un accès interne à l’organisation au travers d’un VPN.
-	Externe, sans VPN : les auditeurs effectuent les tests à distance sans aide pour accéder au réseau. 

### La méthodologie d’un test d’intrusion : la cyber kill chain

La mission est divisée en 4 étapes qui permettent une approche progressive et détaillée du système visé :
Décrire Cyber kill chain
https://upload.wikimedia.org/wikipedia/commons/c/c2/The_Unified_Kill_Chain.png

-	Pour la phase de reconnaissance, les auditeurs lancent les tests sans aucune information de première main, n'ayant qu'un accès physique au réseau interne du SI visé (prise réseau ou accès VPN). Ils identifient les flux de données accessibles à partir de cet accès ou de cette prise.
-	La phase d'acquisition est une série de tests où, pour chaque hôte, le système d'exploitation ainsi que les services visibles de l'extérieurs sont identifiés.
-	Pendant la phase d'exploitation, les auditeurs recherchent les vulnérabilités ou les erreurs de configuration identifiées ou potentielles, qui ont un impact sur le niveau de sécurité du SI. Si une vulnérabilité est identifiée, les auditeurs effectuent des tests en émulant l'exploitation des vulnérabilités. Si l'exploitation peut conduire à une interruption de service, les auditeurs doivent avoir l'accord préalable du client.
-	Pendant la phase de post-exploitation, les auditeurs déterminent des cibles stratégiques qui sont accessibles du point de vue atteint pendant la phase d'exploitation, et émulent les actions qu'un attaquant pourrait prendre lorsqu'il a obtenu un accès non autorisé au SI visé.

### Test d’intrusion physique

Le test de pénétration physique vise à mesurer la force des contrôles de sécurité physique existants et de découvrir leurs faiblesses avant que des acteurs malveillants ne puissent les découvrir et les exploiter. Les tests d'intrusion physique révèlent les possibilités réelles pour les attaquants de pouvoir compromettre les barrières physiques (par exemple : serrures, capteurs, caméras) de manière à permettre l'accès physique non autorisé aux zones sensibles, ce qui conduit à des violations de données et à la compromission du système/réseau.

### Test d’intrusion réseau

Le test d’intrusion réseau vise à identifier les vulnérabilités exploitables des réseaux, des systèmes, des hôtes et des dispositifs de réseau (c'est-à-dire les routeurs, les commutateurs) avant que les attaquants ne puissent les découvrir et les exploiter. Le test de pénétration du réseau révélera les possibilités réelles qu'ont les attaquants de compromettre les systèmes et les réseaux de manière à permettre l'accès non autorisé à des données sensibles ou même de prendre le contrôle de systèmes à des fins malveillantes ou non commerciales.

### Test d’intrusion des applications (système, web et mobile)

Le test de pénétration des applications est d'identifier les vulnérabilités exploitables des applications avant que les attaquants ne puissent les découvrir et les exploiter. Le test d’intrusion des applications révélera les possibilités réelles qu'ont les attaquants de pouvoir compromettre les applications de manière à permettre l'accès non autorisé à des données sensibles ou même de prendre le contrôle de systèmes à des fins malveillantes/non commerciales.

## Audits de sécurité 

### Audit de code
L'audit de code vise à obtenir une vision globale de la sécurité des SI, ainsi que des processus de sécurité normalisés. C'est une évaluation et une vérification de la sécurité du code d'un projet, sur le plan technique et fonctionnel. Cela permet également d'optimiser la maintenance continue pour les équipes de développement, de mieux comprendre les risques associés, et de rendre compte des recommandations applicables lors du développement.
La mission est divisée en 3 étapes qui permettent une approche progressive et détaillée du système visé :
-	Pour la phase d'indentification, les auditeurs se renseignent sur les spécifications fonctionnelles et techniques, en ciblant le module de code critique pour l'application.
-	La revue automatique est une série de tests pendant laquelle les auditeurs lancent de multiples outils d'analyse statique de code. Cette phase peut produire des faux-positifs, ce qui implique la phase suivante :
-	La revue manuelle permet de confirmer les vulnérabilités trouvées lors de l'étape précédente, mais aussi d'effectuer des tests plus précis et adaptés à l'application auditée.

### Audit de configuration

L'audit de configuration vise à vérifier la mise en œuvre des meilleures pratiques en la matière de sécurité et d'audit interne pour la configuration du matériel et des logiciels. Un audit de configuration évalue :
-	Les équipements réseau
-	Les systèmes d'exploitation
-	Les produits de sécurité
-	Les applications
La mission est divisée en 3 étapes qui permettent une approche progressive et détaillée du système visé :
-	Lors de la phase d'indentification, les auditeurs se renseignent sur les services et les flux, la topologie réseau ainsi que les interactions entre équipements
-	Durant la phase d'extraction, toutes les données de configuration sont cherchées et regroupées pour être analysées.
-	La phase d'analyse permet de trouver des failles dans la configuration de l'authentification, des mécanismes cryptographiques, des règles de filtrage réseau, ainsi que celle des serveurs et infrastructures.

### Audit d’environnement virtualisé

Une évaluation de la sécurité de la virtualisation d’un système d’information est une étude visant à identifier les vulnérabilités et les risques en matière de sécurité informatique dans un environnement virtualisé : serveurs physiques, dispositifs réseaux connectés, types d’hôtes virtuels, configurations de réseaux réels et virtuels, contrôles d’accès…

### Audit d’Active Directory

TODO

### Audit d'infrastructure

Un audit d'infrastructure vise à évaluer un système d'information dans le but de déterminer ses forces et faiblesses. Cela pourra aider l'organisation à établir une vue globale sur son réseau, pour qu'elle prenne les meilleures décisions stratégiques au sujet de la sécurité informatique. 
La mission est divisée en 11 étapes qui permettent une approche progressive et détaillée du système visé :
-	Rencontre de l'équipe qui gouverne le système d'information
-	Identification de la structure du département informatique
-	Evaluation de la sécurité de l'organisation
-	Prise en compte des plans de reprise après sinistre
-	Etude du réseau interne de l'organisation
-	Evaluation des serveurs 
-	Analyse de la sécurité du stockage des données et du chiffrage cryptographique
-	Etude de la communication au sein de l'organisation
-	Evaluation de la sécurité des terminaux (par exemple les PC des employés)
-	Etude des applications utilisées
-	Analyse du support informatique de l'organisation 

### Audit d’architecture d’une application

L'objectif d'un audit d'architecture est de vérifier la cohérence fonctionnelle et la conformité d’une application, de manière moins détaillée et moins longue qu’un audit de code, mais plus profonde qu’un audit de configuration.
La mission est divisée en 3 étapes qui permettent une approche progressive et détaillée du système visé :
-	Phase d'identification : les auditeurs découvrent les spécifications fonctionnelles de l’application à auditer, ainsi que son architecture.
- TODO

### Audit d’infrastructure Wi-Fi

L'audit de la sécurité des réseaux sans fils permet d'identifier les vulnérabilités et les risques de sécurité dans le réseau sans fil. 
La mission est divisée en 3 étapes qui permettent une approche progressive et détaillée du système visé :
-	Phase d'identification : les auditeurs découvrent la topologie du périmètre à auditer, et possiblement des points d'accès malveillants mis en place.
-	Pendant la phase d'exploitation automatique, des outils de brute force sont utilisés pour tenter de briser les mécanismes d'authentification des protocoles utilisés.
-	Durant la phase d'exploitation manuelle, les auditeurs mettent en place des faux points d'accès afin d'induire en erreur des employés pour découvrir des informations sensibles.

## Ingénierie sociale

Une évaluation de la sécurité par ingénierie sociale est un outil très performant pour comprendre l'exposition aux menaces de la sécurité de la plupart des organisations. Les êtres humains étant généralement le maillon faible de toute la stratégie de sécurité, ce travail permet d'identifier rapidement les domaines qui doivent être traités le plus rapidement possible. Un autre facteur dont il faut tenir compte est que les êtres humains peuvent également être très imprévisibles, selon les circonstances dans lesquelles ils se trouvent. C'est pourquoi les auditeurs sont capables de concevoir, organiser et mener à bien une évaluation de ce type.

### La méthodologie d’une approche d’ingénierie sociale

Une évaluation de la sécurité par ingénierie sociale passe par 3 phases :
-	La phase de reconnaissance est cruciale au bon fonctionnement d'une évaluation de la sécurité. Les auditeurs recherchent toute information publique qui est utile au développement de scénarios d'attaques pertinents. De l'information privée, peut également être fournie par l'organisation auditée. 
-	Phase de création de scénarios : une fois que l'organisation a bien été identifiée, les auditeurs formulent des scénarios d'attaques (informations nécessaires au scénario, prétexte, objectif) afin de pénétrer le système ou l'organisation.
-	Durant la phase d'engagement, en utilisant les tactiques et prétextes spécifiés, les auditeurs commencent par une campagne de courriels malveillants ou d'appels téléphoniques appropriés. Pour les évaluations sur site, une série de tests est lancée, y compris la filature et le "baiting", pratique visant à laisser des clés USB sur le sol de zones communes (comme un parking).

### Audit de renseignement de source publique (OSINT)

L’audit OSINT vise à découvrir le plus d’information possible, de préférence sensible, à propos d’une organisation ou d’une personne, seulement à l’aide de sources publiques, tels qu’un moteur de recherche et les réseaux sociaux.

### Campagne d’hameçonnage par email (phishing)

La campagne d’hameçonnage par email vise à s’introduire dans le système d’information d’une organisation à partir d’emails frauduleux personnalisés, diffusés au plus grand nombre de collaborateurs.

### Campagne d’hameçonnage par SMS (smishing)

La campagne d’hameçonnage par SMS vise à s’introduire dans le système d’information d’une organisation à partir de SMS frauduleux personnalisés, diffusés au plus grand nombre de collaborateurs. 

### Ingénierie sociale par téléphone (Vishing)

Les auditeurs peuvent accéder à des informations privées en se faisant passer pour un collaborateur, et ainsi outrepasser des mesures de sécurités telle que l’authentification.

### Clés USB leurres

Les auditeurs déposent sur des zones publiques communes, telles qu’un parking, une cafeteria d’entreprise, des clés USB malveillantes qui peuvent donner accès au système informatique interne et voler des informations sensibles lorsqu’un collaborateur en connecte une à sa machine professionnelle.

## Exercices de sécurité

TODO

### Red team

### Purple team

### Simulation d’APT

### Cybermalveillance

### Cyberattaque

## Audits de prévention ou post-incidents

### Investigation numérique

L'investigation numérique est l'ensemble des connaissances et méthodes qui permettent de collecter, conserver et analyser des preuves issues de supports numériques en vue de les produire dans le cadre d'une action en justice, et de retracer l'activité malveillante d'un attaquant.
L'investigation suit un processus en 4 étapes :
-	Acquisition : les auditeurs collectent les preuves avec l'approbation des autorités
-	Identification : les preuves sont transformées du format numérique à un format compréhensible par l'homme.
-	Evaluation : les auditeurs déterminent l'exactitude des preuves recueillies, et si elles peuvent être considérées comme pertinentes pour le cas faisant l'objet de l'enquête
-	Admission : tous les éléments de preuve extraits sont présentés.

### Audit de logiciels malveillants

L'audit de logiciel malveillants vise à détecter tout logiciel malveillant qui est ou qui a été présent sur le système d'information audité. Une analyse de l'impact sur le système d'information et sur l'organisation est alors menée pour chaque logiciel suspicieux découvert.
La mission est divisée en 3 étapes qui permettent une approche progressive et détaillée du système visé :
-	Phase d'identification du système informatique : les auditeurs découvrent le périmètre à inspecter.
-	Durant la phase de détection, des scans automatiques sont lancés afin de trouver des traces d'exécution d'une activité suspicieuse ou de logiciels malveillants connus.
-	La phase de nettoyage met en place un processus d'élimination des logiciels malveillants découverts afin de rendre sain le système d'information.
-	Pendant la phase de recherche, les auditeurs qualifient l'impact provoqué par chaque logiciel découvert.
