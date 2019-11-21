# SR06 Module 1 - Risques

## Démarche

### Motivation

Pour une entreprise, la motivation est une perte de richesse par:

* Incapacité à produire
	* Système endommagé
	* Système de production hors service
* Vol d'information
	* Gain à la concurrence
	* Atteinte à l'image de l'entreprise
* Condamnation judiciaire
	* Non respect des dispositions légales
	* Attaques d'un tiers par rebond
* Confiance des clients et des partenaires

Pour un particulier, plutôt respect de la vie privée, pas d'usurpation d'identité...


Immobilisation dans les normes IRFS *(présentation des états financiers d'une entreprise)*

Accord de Bâle *(une banque ne doit pas prêter plus qu'un certain niveau)*

Protection des actifs de l'entreprise *(+- 2/3 immatériels)*

* Fichiers, bases de données...

Le système d'information est donc **partie prenante** des actifs. Il faut évaluer le risque.

### Méthode générale

Trois phases principales dans la gestion d'une crise :

* Avant : *prévention* : **analyse de risques**
* Pendant : *action*
* Après : *récpération*

1. Connaître le système.
	* Nouveau projet : **sécurité par la conception**
	* Système existant : **Analyse du système**
2. Évaluation de risques
	* **Analyse** du risque
	* **Gestion** du risque
3. Exprimer des besoins
	* Définir les **exigences** de sécurité
	* Établir une **Politique de Sécurité** du SI *(PSSI)*
4. Déployer des mesures
	* Organisationnelles
	* Techniques

Quoi sécuriser ? Ne pas se focaliser sur un point particulier *(ex : réseau)* sans avoir une vision d'ensemble.
Inclure les processus de l'entreprise, les employés.

*Ex: à quoi bon protéger le réseau si le risque d'inondation où de panne de clim est plus grand ?*

=> Quels risques prendre en compte ? *Sécurité à 100% existe ?*

* **Diminution** du risque :
	* Modification du système
	* Déploiement de mesures organisationnelles et techniques
* **Suppression** du risque :
	* Risque trop important pour l'entreprise
	* Suppression de la partie du système qui présentait ce risque
* **Transfert** de risque :
	* Risque difficile à éliminer
	* Assurance auprès d'un tiers
	* revient à transférer le risque au tiers *(comme une assurance incendie)*

Méthode de gestion :

1. Identification
2. Hiérarchisation
3. Définition d'un seuil d'acceptation
4. Traitement des risques non acceptables

**Traitement** des risques :

* Définition des objectifs de sécurité
* Application de mesures techniques ou organisationnelles.

La démarche est un cycle continu :

* Travail long, sujet à des améliorations successives
* La sécurité est un **éternel recommencement** : évolution des SI, évolution des menaces.

Méthode **Plan Do Check Act** (PDCA) : amélioration cyclique continue.

## Analyse du SI

Délimiter ce qu'il faut protéger, connaître ce qu'il faut protéger

### Principe

* Considérer l'**information** au sens large, dans ses trois états :
	* Traitement, Stockage, Transport
* Considérer **tous les supports** :
	* Non informatisés : parole, papier
	* Informatisé : processeur, réseaux, supports de stockage
* Considérer **tous les actifs** concernés

Mise en place d'un **inevntaire** :

* Exhaustif
* Méthodique : **délimiter**, **décomposer**, **cartographier**

**Cycle de vie** d'un **actif** :

* Création
* Utilisation
* Stockage
* Fin d'utilisation

**Gestion** par la supervision du cycle de vie et sécurité à chaque étape.

### Méthode

1. **Délimiter** le contour du système
2. **Décomposer** le système en actifs
3. **Cartographier** le système et ses parties
4. Enquête auprès des utilisateurs

**Décomposition** du SI :

* Les personnes *(sécurité, management, visiteurs...)*
* Les logiciels *(sécurité, administration...)*
* Les matériels

Connaissance du SI ?

* Qui connaît l'ensemble du SI ?
* Qui connaît l'architecture logicielle ?
* Qui connaît le plan des réseaux ?

Difficultés à prendre en compte :

* Shadow IT
* Informatique externalisée...

Usage du Smatphone personnel :

* Accompagne l'employé au travail et dans ses déplacements
* Fait souvent office de téléphone professionnel
* Souvent connecté au PC pour le recharger

Problème : un **pont** entre professionnel et personnel !

Attention aux informations sensibles :

* Secret commercial, faille de sécurité, version d'un logiciel, fournisseur, information RH...
* Respect de la déontologie, de la charte de confidentialité

Outils en ligne ?

* Données confidentielles qui échappent à l'entreprise
* Confiance dans les outils et leurs propriétaires

## Analyse des risques

### Définition

Le **risque** est l'espérance mathématique d'une fonction de probabilité d'événement

Calcul :

$`alea_i = p_i \time c_i`$
$`risque = \sum{i}{}alea_i`$

ISO 2010 : Le risque est l'effet de l'incertitude sur l'efficacité de l'organisation

**Risque absolu** : menaces x vulnérabilité
**Risque réel** : risque absolu x sensibilité

*Ex réduction du risque pour un magasin :*

* Réduire la **menace** :
	* S'installer là où il y a moins de voleurs
	* Baisser les prix
	* Faire « profil bas » *quand c'est possible*
* Réduire la **vulnérabilité** :
	* Renforcer la vitrine, rideau de fer...
	* Caméra
* Réduire la **sensibilité** :
	* Prendre une assurance
	* Ne pas exposer toute la marchandise
	* Mettre au coffre les pièces rares

### Menace

C'est la cause potentielle d'un incident. Elle pourrait entraîner des dommages si elle se concrétisait.

Classement selon l'ampleur *(grande ampleur, ampleur localisée, atteinte partielle)* et l'origine *(volontaire, accidentelle, naturelle)*

Considérer les 4 menaces principales :

* **Interception**
* **Injection**
* **Modification**
* **Interruption**

Quel niveau ? Réseau ? Applicatif ? Organisationnel ?

### Vulnérabilité

C'est la faiblesse au niveau d'un actif.

**Attaque** action malveillante résultant de la réalisation d'une menace et de l'exploitation d'une vulnérabilité

Conception, réalisation, installation, configuration, utilisation, fin de vie.

### Sensibilité

Mesure de l'impact d'une attaque. Tout actif ne mérite pas la même attention.

Faut-il chiffrer tous les supports ?
Faut-il seconder toutes les alimentations électriques ?

Permet de hiérarchiser les actions à mener.

Les procédures n'ont un intérêt que si elles sont **efficaces**

* « Trop de sécurité nuit à la sécurité »
	* Inefficacité technique
	* Surcout
	* Contournement
* Conditions d'une démarche efficace
	* Réalisable
	* Comprise et acceptée de tous
	* Appliquée
* La sécurité à 100% existe t elle
	Le SI doit rester utilisable

## Exigences de sécurité

Il s'agit du besoin intrinsèque de sécurité pour un actif.
Aussi appelé **besoin de sécurité**.

Pour satisfaire ces exigences, on déploiera des **fonctionnalités de sécurité**, avec un définition de **métrique** pour chaque exigence.

Ex :

* L'exigence de confidentialité est plus forte pour le code source que pour le code objet
* L'exigence d'intégrité est plus forte pour le code objet que pour le code source

=> Chiffrement du PC du développeur
=> Emprunte SHA pour l'exécutable

Autres points :

* **Auditabilité** : exigence à part entière ou moyen de vérifier les autres exigences ?
* **Authentification** : exigence de sécurité ?

En SR06, auditabilité, preuve $`\in`$ exigences, authentification $`\in`$ fonctionnalités

### Confidentialité

L'information n'est accessible que par les seules personnes autorisées :

* Inutile de chiffrer une information publique
* Ne pas divulguer son mot de passe

Contraintes légales :

* Données personnelles
* RGPD

Modèle de Bell et LaPadula :

1. *No read-up* : ne pas lire des niveaux de confidentialités supérieurs
2. *No write-down* : ne pas écrire dans des niveaux de confidentialité inférieurs

Rq :

* Difficile d'augmenter la confidentialité
* Impossible si l'information était publique

Exemple pour une entreprise :

1. Public
2. Restreint
3. Confidentiel avec les partenaires
4. Confidentiel interne
5. Secret

### Intégrité

L'information ne doit être modifiée que par les personnes autorisées, et uniquement le la manière autorisée.
Propriété d'exactitude et de complétude de l'information

Exemples d'utilisation :

* Intégrité faible sur les annuaires téléphoniques
* Intégrité forte sur les plans de fabrication.

Modèle de Biba :

1. *No read-down*
2. *No write-up*

Ex pour un support papier :

1. Feuille normale
2. Feuille + cachet et signature
3. Support non photocopiable
4. Support augmenté de systèmes difficilement reproductibles
5. Support à usage contrôlé

Pour un support informatique :

1. Aucune mesure
2. Somme de contrôle
3. Fonction de hachage
4. Partition non inscriptible
5. Support non modifiable

### Disponibilité

L'actif doit être opérationnel au moment où l'on en a besoin, et avec la qualité de service attendue.

Ex:

* L'indisponibilité d'un serveur est plus problématique que celle d'un poste client
* Impact des architectures centralisées, des réseaux

Métriques de disponibilité :

* Contractuel en cas d'externalisation d'un service
* Délai d'indisponibilité maximal accepté
* Nombre d'incidents simultanés pour rendre inopérant le service

### Preuve

Les actions réalisées sur un actif sont connues

Permet de s'assurer du respect des autres exigences. Principe de contrôle aux transitions entre zones de contrôle.
Investigations légales

Ex :

* Registre des visiteurs
* Logs de connexions
* Les logs d'un PC sont moins complets que ceux d'un pot de mail.

Limites légales :

* Type d'information
* Durée de conservation
* Attention aux transferts transfrontaliers

Limites techniques :

* les logs ne sont utiles que si on les exploite => ressources
* Intégrité crutiale

**Imputabilité** : attribuer la responsabilité d'une action à une personne

Croisement entre auditabilité/preuve et Identification/Authentification

## Politique de sécurité

La **politique de sécurité** est l'ensemble des exigences de sécurité et des procédures permettant de s'y conformer

* Document présentant les exigences de sécurité et les consignes pour les personnels
* Valeur obligatoire

Exploiter l'analyse de risques :

* Sélectionner les exigences adéquates
* Mettre en face les **fonctionnalités** de sécurité appropriées

Mise en application

Impliquer la direction de l'entreprise

**Fonctionnalité de sécurité :** brique technologique ou méthodologique permettant de répondre à une exigence.

Référentiels :

* Norme ISO 27002 et 27005

Méthodes :

* Ebios
* Mehari
* ANSSI

Exigences des informations :

* **Sensibles** : peut porter atteinte si divulguées
* **Vitales** : nécessaires au bon fonctionnement
* **Stratégiques**
* **Nomminatives**
* **Coûteuses**
