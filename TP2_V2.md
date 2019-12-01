## Projet du module Cryptographie * [ ]  Automne 2019

Le **projet du module Cryptographie** consiste à compléter le système informatique issu du projet précédent (module Risque) avec une sécurisation des échanges. Cette architecture sera encore complétée lors du projet du module Protection.

Pour rappel, l'architecture du SI est composée de trois machines virtuelles en réseau :

* [x]  un serveur Linux servant des pages web via Apache/PHP ;
* [x]  un client Linux connecté au serveur par un premier réseau, vmbr0 (supposé être le LAN de l'entreprise) ;
* [x]  un client Windows connecté au serveur par un second réseau, vmbr1 (supposé être l'accès Internet de l'entreprise).

Remarque : Le client Windows peut avantageusement être remplacé par un clone du client Linux pour économiser de l'espace sur Proxmox.

Le but du projet est de :

1. mettre en place une connexion sécurisée automatique pour les utilisateurs *publie* et *edite* depuis le client Linux sur le serveur Linux ;
2. mettre en ligne sur le serveur web un "contrat" signé ;
3. déployer TLS/SSL dans le serveur web en utilisant un des deux logiciel suivants :
   - XCA ([https://hohnstaedt.de/xca/](https://antispam.utc.fr/proxy/2/QmVydHJhbmQuRHVjb3VydGhpYWxAdXRjLmZy/hohnstaedt.de/xca/) recommandé)
   - TinyCA 2
4. et, optionnellement, exploiter cette PKI pour les étapes 1 et 2.

**Partie 1 : connexion sécurisée automatique**

* [x]  Depuis un compte du client Linux, créer des bi-clés (différentes) pour les utilisateurs *edite* et *publie* avec ssh-keygen ;
* [x]  Utiliser ssh-copy-id pour transférer les clés concernées sur le serveur ;
* [x]  Vérifier ensuite que les utilisateurs *edite* et *publie* peuvent se connecter automatiquement au serveur et réaliser les actions définies dans le projet Risque.

**Partie 2 : contrat signé**

* [ ]  Créer un texte quelconque avec le logiciel de votre choix, sauvegardé dans un fichier appelé *contrat* ;
* [ ]  Créer une bi-clé RSA en utilisant open-ssl ;
* [ ]  Signer le contrat avec la bi-clé en utilisant openssl ;
* [ ]  Mettre en ligne (sur le serveur web) le contrat et la clé permettant de vérifier sa signature ;
* [ ]  Vérifier qu'un utilisateur distant puisse vérifier l'authenticité du contrat.



**Partie 3 : PKI**

3.1 Chaîne de confiance

Mettre en oeuvre une chaîne de confiance permettant d’émettre des certificats :

* [ ]  D’authentification serveur (permettant de mettre en place le SSL/TLS sur un serveur web);
* [ ]  D’authentification client (permettant de mettre en oeuvre de l’authentification du client sur un serveur web).

La chaine de confiance devra respecter les contraintes suivantes :

* [ ]  Chaine de confiance 3 tiers (3 niveaux d’AC);
* [ ]  Ségrégation par usage, l’authentification d’un client étant considéré comme un usage différent de l’authentification d’un serveu).

3.2 Utilisation

Sur le serveur web, mettre en oeuvre :

* [ ]  Un vhost avec le module SSL actif et correctement configuré (aussi bien sur le navigateur qui servira pour la démonstration côté client que pour le serveur) ;
* [ ]  Une authentification du client par certificat ;
* [ ]  Un contrôle de CRL dans la configuration du serveur web ; vérifier qu’il n’est plus possible d’authentifier un client possédant un certificat révoqué.

Pour rappel :

* [ ]  Un profil de certificat d’AC doit contenir les éléments suivants :

- * [ ]  Key Usage: CRL Sign, Certificate Sign (critical)
  * [ ]  Extended Key Usage : aucun
  * [ ]  Basic Constraint: CA = TRUE

* [ ]  Un profil de certificat pour de l’authentification serveur doit contenir:

- * [ ]  Key Usage: Digital Signature, Key Encipherment (critical)
  * [ ]  Extended Key Usage: Server Auth
  * [ ]  Basic Constraint: Aucun

* [ ]  Un profil de certificat pour de l’authentification client doit contenir :

- * [ ]  Key Usage: Digital Signature, Key Encipherment (critical)
  * [ ]  Extended Key Usage: Client Auth
  * [ ]  Basic Constraint: Aucun



**Partie 4 : Optionnelle**

Refaire les parties 1 et 2 en exploitant/complétant la PKI définie à l'étape 3.