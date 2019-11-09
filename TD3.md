Ces actions sont à réaliser sur la machine virtuelle Windows 10 'sr06-win-10-1'

# I. Préparation et premier démarrage de la machine virtuelle Windows 10


* [x]  Localisez la machine virtuelle "Windows 10" fournie ('sr06-win10-1') :


* [x]  Clonez la machine virtuelle ('Full clone')

* [x]  Dans les préférences stockage de la machine virtuelle, sélectionnez 'Hard Disk' et ajoutez un nouvel espace de stockage virtuel disque dur de 10 Go

* [ ]  Démarrez la machine virtuelle Windows

* [x]  Ouvrez la session locale "formation" avec le mot de passe "formation"

* [ ] Sélectionnez l'icône du bureau "Ce PC", et par un clic droit dessus, affichez ses propriétés

* [ ] Renommez le nom de l'ordinateur en "W10" et le groupe de travail en "SR06-WIN10". Redémarrez ensuite la machine virtuelle comme demandé par Windows


     * [x] Erell

* [x] Sur l'icône du bouton "Démarrer", faites un clic droit et sélectionnez "Gestion de l'ordinateur", puis "Gestion des disques"	

 * [x] Initialiser le "Disque 1" avec une partition "GPT" (GUID Partition Table)

      ​	boot pour démarage

      ​	pas trouver le Disque 1.... -> et reinitialiser et redémarrer

 * [x] Sélectionnez la zone non allouée du "Disque 1" et créez un nouveau "Volume simple" couvrant la taille de la zone non allouée, avec la lettre de lecteur "S", le système de fichier NTFS et le nom de volume "Stockage"

* [x] Quittez le gestionnaire de disques et double-cliquez sur l'icône du bureau "Ce PC". Constatez la présence du nouveau disque dur "Stockage (S)"

  un stockage(S) -> 9.94G0

### Connaissance:

1. GPT
2. Système de fichier
   1. NTFS
   2. 


# II. Sécurisation de base

* [x]  Dans la barre de tâches, zone de notifications, vérifiez la présence de l'antivirus "Trend Micro OfficeScan"

* [x] Vérifiez les versions de ses composants et notamment ceux concernant les signatures

  15.403.00

* [x] A quel serveur maître est attaché ce client antivirus. Est-ce un serveur Internet ou un serveur de l'établissement. A votre avis, pourquoi un tel choix ?

  Un serveur local

* [x]  Dans le doute, lancez une mise à jour de l'antivirus

    Qu'est-ce qu'un bon antivirus et un bon antimalware selon vous et quelle est sa bonne utilisation ?

​		1. Régulièrement à jour tout seul

​		2. 

* [x]  Dans le menu "Démarrer", sélectionnez "Paramètres" (icône de la roue crantée), puis "Mise à jour et sécurité" et "Windows Update"

    Les mises à jour sont automatiques sur Windows 10. Elles peuvent être délivrées par un serveur local à l'établissement (serveur WSUS) où celui-ci pourra filtrer les mises à jour à délivrer au postes clients Windows

​		UTC a un serveur WSUS

* [x] Forcez la mise à jour en sélectionnant "Rechercher les mises à jour"

* [x] Sélectionnez "Affichez l'historique des mises à jour". Cliquez sur l'un des correctifs de cette liste pour en connaître les détails

* [ ] Revenir sur "Windows Update" et sélectionnez "Options avancées", puis "Optimisation de distribution". Selon vous, vaut-il mieux activer ou désactiver la fonctionnalité "Autoriser les téléchargements à partir d'autres PC" ?

  Desactiviter

* [ ]  Revenir sur "Mise à jour et sécurité" et sélectionnez "Sauvegarde"

* [ ] Il est possible d'activer une sauvegarde régulière de vos fichiers par la fonction "Historique des fichiers". C'est un filet de sécurité simple à mettre en œuvre et dont le bénéfice est immense

 * [ ] Sélectionnez "Ajouter un lecteur" et sélectionnez le lecteur proposé qui doit être "Stockage (S)"
   
* [x] Erell
  
* [ ] Sélectionnez "Plus d'options" et prenez connaissance de la liste des des dossiers sauvegardés par cette méthode. Que pensez-vous de cette liste par défaut ?
  Bien car il y a tout ?

* [ ] Sur le bureau, créez un dossier "Data", puis, dedans un fichier texte "Versions.txt". Editez ce fichier et saisissez la chaîne de caractères "Version 1" puis enregistrez ce changement
  
* [x] Erell
  
* [ ] De retour dans "Plus d'options" (Options de sauvegarde), Sélectionnez "Sauvegarder les données maintenant" et attendre la fin de la sauvegarde. La date de la dernière sauvegarde s'affiche alors
  
* [x] Erell
  
* [ ]    De retour sur le bureau, rééditez le fichier "Data\Versions.txt" et remplacez la chaîne de caractères "Version 1" par "Version 2" puis enregistrez ce changement
  
* [x] Erell
  
* [ ]    Sélectionner l'icône du fichier "Versions.txt" et, par un clic droit, sélectionnez "Restaurer les versions précédentes", puis le fichier "Versions" qui est listé. Restaurez-le sur le bureau, ouvrez-le et constatez qu'il contient bien la chaîne de caractères "Version 1"
  
* [x] Erell
  
* [ ]    De retour dans "Plus d'options" (Options de sauvegarde), quelle valeur de l'option "Conserver mes sauvegardes" choisiriez-vous personnellement et pourquoi ?
J'aurai gardé la même, mêlme "pour toujours" on ne sais pas jusqu'à quand c'est

7. Dans la barre de tâches, zone de notifications, lancez le "Sécurité Windows"

    "Sécurité Windows" présente un tableau de bord synthétique des services dédiés à la sécurité et au bon fonctionnement du système

* [ ]    Constatez que tous les voyants sont au vert

    Erell - Pas pour moi : pas la "protection du compte" ni les "performances et intégrité de l'appareil"

8. Dans "Sécurité Windows", sélectionnez "Protection du compte" et "Gérer les options de connexion", puis, dans "Options de connexion", modifiez le mot de passe de votre compte.

    Attention à bien choisir et à bien retenir ce mot de passe. Il ne sera pas possible de le retrouver une fois en place. Et heureusement !

    Qu'est-ce qu'un bon mot de passe selon vous et quelle est sa bonne utilisation ?

- Long, sans mot du dico, comportant des caractères spéciaux
- un mot de passe différent pour chaque chose (pas utilisé deux fois)
- en changer régulièrement
- pas gardé quelque part d'autre que la tête du propriétaire 

9. Les mots de passe de Windows (poste hors domaine Active Directory) sont stockés dans le fichier SAM (Security Account Manager), dans "C:/Windows/system32/config/". Ils sont cryptés en NTLMv2.

* [ ]    Allez dans le dossier "C:/Windows/system32/config/" et copiez/collez le fichier "SAM" sur le bureau. Que constatez-vous ?

    "Cette action ne peut être réalisée car le fichier est ouvert dans System"

    Allez sur Internet et faites une recherche sur "John the ripper" et/ou "Hash suite". Que permettent ces outils spécialisés ? Que faut-il en retenir concernant le choix d'un bon mot de passe ?
    
 Ils permettent de cracker des mots de passe
 Je ne sais pas 

10. Dans "Options de connexion", désactivez l'option de "Confidentialité" permettant de terminer automatiquement la configuration après une mise à jour avec redémarrage. Pourquoi vaut-il mieux désactiver cette option, selon vous ?
  * [x] Erell
 je ne sais pas non plus

11. Dans "Options de connexion", sélectionnez "Ecran de verrouillage" (paragraphe "Paramètres associés")

    Désactivez l'affichage de l'état de toutes les applications sur l'écran de verrouillage
  * [x] Erell

    Réglez l'écran de veille pour qu'il se déclenche au bout de 2 minutes et, qu'à son réveil, une authentification d'ouverture de session soit exigée. Selon vous, pourquoi activer cette fonctionnalité est-elle importante ?
  * [x] Erell
Si l'on oublie d'éteindre notre ordinateur au bout de peu de temps il se vérouille de lui-même.

    Testez le bon fonctionnement de ces réglages

    Pour des raisons pratiques liées à ce TD, réglez, de nouveau, l'écran de veille pour qu'il ne se déclenche qu'au bout de 5 ou 10 minutes

12. Dans "Sécurité Windows", sélectionnez "Pare-feu de protection du réseau"

    Vérifiez que le pare-feu est activé
  * [x] Erell

    Vérifiez que les notifications du pare-feu sont activées
  * [x] Erell

    Sélectionnez "Paramètres avancés", constatez l'affichage de la fenêtre d'alerte du "Contrôle de compte utilisateur" (UAC). Quelles informations importantes sont présentées à l'utilisateur et dans quel but ?
    "voulez-vous autoriser cette application à apporter des modifications à votre appareil ?"
    "Editeur vérifié : Microsoft Windows"
    "CLSID : {3213CD15-4DF2-415F-83F2-9FC58F3AEB34}"
    Tout pour vérifier quelle est l'applicatione qui va modifier le PC et d'où elle provient

    Sélectionnez "Analyse". Dans le paragraphe "Etat du pare-feu", notez les règles, par défaut, d'application des règles entrantes et sortantes. Que pensez-vous de la règle d'application par défaut des connexions sortantes ?
    "Les connexions entrantes qui ne correspondent pas à une règle sont bloquées"
    "Les connexions sortantes qui ne correspondent pas à une règle sont autorisées"
    --> il est moins dangereux que ce soit les sortantes qui soient autorisées que les entrantes mais ça reste dangereux je pense, non ?

    Quittez l'application pare-feu Windows

13. Dans "Sécurité Windows", sélectionnez "Contrôle des applications et du navigateur"

    La protection "Windows Defender Smartscreen" consulte des listes centralisées de sites web, d'applications et de fichiers reconnus comme malveillants, ainsi que sur le comportement suspicieux de certaines pages Web

    Vérifiez que les protections contre le téléchargement de fichiers et d'applications, et contre les sites Web malveillants sont configurées sur "Avertir" ou "Refuser"
    ils sont en "avertir"

    Sélectionnez "Exploit protection" et vérifiez que toutes les protections de ce type sont à leur valeur par défaut. Ces protections sont très importantes et permettent, au travers de mécanismes très bas niveau, voire même matériels comme la protection PED (DEP, Data Execution Prevention), de se prémunir de la plupart des attaques effectuées sur le code des applications
  * [x] Erell

    Faites une recherche Internet sur "DEP" et expliquez pourquoi est-ce important d'activer cette fonctionnalité
    https://docs.microsoft.com/en-us/windows/win32/memory/data-execution-prevention

14. Dans "Sécurité Windows", sélectionnez "Sécurité des appareils"

    Ici nous ne voyons que la partie concernant l'isolation du noyau grâce à des fonctions de virtualisation

    Cependant, d'autres protections seront listées ici si le processeur dispose d'un module TPM (Trusted Platform Module, cryptage) ou si le système d'exploitation est démarré via la fonctionnalité Secure Boot

Faites une recherche sur Internet concernant "Secure Boot" et expliquez contre quelles programmes malveillants elle permet de se prémunir
    
https://docs.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-secure-boot
    


\15. Sur le bureau, double-cliquez sur l'icône "Ce PC", puis sur l'onglet "Affichage". Sélectionnez "Options" et "Modifier les options des dossier et de recherche"

​    Sélectionnez l'onglet "Affichage" et sélectionnez "Afficher le chemin d'accès complet dans la barre de titre" et "Afficher les fichiers, dossiers ou lecteurs cachés, puis désélectionnez "Masquer les extensions des fichiers dont le type est connu". Selon vous, pourquoi faire ces choix d'options d'affichage est-il important ?

Le "Masquer les extensions des fichiers dont le type est connu" était déjà décoché pour moi.

Afficher le chemin d'accès est intéréssant pour savoir où l'on est exactement. Il est aussi important  de pouvoir voir les fichiers cachés et d'avoir les extension des fichiers car ça permet de savoir ce qu'ils sont. ?

\16. Sur l'icône du bouton "Démarrer", faites un clic droit et sélectionnez "Gestion de l'ordinateur", puis "Gestionnaire de périphériques"

​    Assurez-vous que tous les pilotes de périphériques fonctionnent normalement.

Tous OK sauf Périphérique PCI : "Les pilotes de ce périphérique ne sont pas installés. Il n'y a pas de pilotes compatibles pour ce périphérique" --> **mettre à jour ?**

​    Dans la liste des périphériques, sélectionnez "Carte réseau" puis "Intel(R) PRO/1000 MT Network Connection" et affichez ses propriétés concernant l'onglet "Pilote". 2 informations listées ici sont importantes concernant la sécurité. Lesquelles selon vous ?

La version et le signataire numérique ? (j'aurai aussi mit la date)

​    Quittez le gestionnaire de périphériques et sélectionnez "Utilisateurs et groupes locaux"

​    Désactivez le compte "Invité". Selon vous, pourquoi est-il important de désactiver ce compte ?

Car il n'est pas sécurisé ?

​    Sélectionnez le compte "formation" et affichez ses propriétés. De quel groupe d'utilisateurs est-il membre ? Quels sont les privilèges attachés aux membres de ce groupe ? Faut-il utiliser ce compte pour une exploitation quotidienne de l'ordinateur ? Pourquoi ?

Il est membre de "Administrateurs".

Les membres du groupe Administrateurs disposent d'un accès complet et illimité à l'ordinateur et au domaine.

Il ne faut pas utiliser ce compte pour une exploitation quotidienne de l'ordinateur car, comme il a tout les droits, un accident est vite arrivé.



​    Sélectionnez le compte "Administrateur" et affichez ses propriétés. De quel groupe d'utilisateurs est-il membre ? Selon vous, pourquoi est-il désactivé ?

Il est aussi membre de "Administrateur".

Il est désactivé car il y a déjà un compte administrateur ?

​    Créez 3 nouveaux utilisateurs non privilégiés "u1", "u11" et "u2" avec un mot de passe identique pour des raisons pratiques. Leur mot de passe ne pourra être changé et il n'expirera pas non plus. De plus si quelqu'un accède au compte, il aura beaucoup moins de possibilités de nuire.

​    Créez 2 groupes d'utilisateurs "g1 et "g2". "g1" aura pour membres "u1 et u11", "g2" aura pour membre "u2"

​    Selon vous, lorsque cette machine est sur le réseau, quels sont les noms complets de l'utilisateur Windows "u1" et du groupe Windows "g2" s'ils doivent être référencés par une autre machine Windows du réseau ?