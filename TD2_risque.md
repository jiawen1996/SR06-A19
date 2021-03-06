## **1. Configuration réseau de la machine virtuelle passoire**

 Nous commençons par adapter la configuration réseau de la machine virtuelle passoire.

### Configuration actuelle

* [x] Consulter la configuration réseau de la machine virtuelle dans la section Configuration/Hardware/Network Device de proxmox.
* [x] Dans la console de passoire, consulter la configuration réseau avec

```shell
ifconfig 
ip address show
```

* [x] Consulter les fichiers `/etc/network/interfaces` et `/etc/netplan/50-cloud-init.yaml`. Que peut-on en déduire ?

![image-20191001161722063](./img/image-20191001161722063.png)

* [x] Vérifier en comparant les commandes suivantes ; expliquer :
  * dpkg -l | grep ifupdown

    dpkg ne trouve pas le package `ifupdown` dans ceux installés

  * dpkg -l | grep netplan
    
    netplan est installé

### Modification de la configuration réseau

* [x] **En consultant `/etc/netplan/50-cloud-init.yaml`, déterminer la méthode utilisée pour l'attribution de l'adresse IP à passoire.**

    Attribution de l'adresse IP par le protocole DHCP4 pour ens18.
* [x] **Relever l'adresse IP actuelle ; comparer avec un binôme voisin. Expliquer.**

```shell
 ip address show
```

* [x] **Consulter `/etc/machine-id` et comparer avec un binôme voisin.**

* [ ] **Modifier l'ID de la machine virtuelle :**

  * [ ] **Supprimer le fichier `/etc/machine-id`.**
  * [ ] **Générer un nouvel ID avec `systemd-machine-id-setup`.**
  * [ ] **Vérifier en consultant à nouveau `/etc/machine-id`. Comparer avec le binôme voisin.**

* [ ] **Renouveler l'adresse IP :**

  ```bash
  sudo dhclient -r
  ```

* [ ] **Vérifier avec `dhclient -v` ou `ifconfig` ou `ip address show`. Comparer avec le binôme voisin.**

* [x] **Suppression de l'interface réseau virb0**

  **Cette interface réseau virtuelle est ajoutée par ubuntu-serveur pour gérer la virtualisation. Elle est inutile ici.**

  * [x] **Enlever l'interface virbr0 temporairement avec  `virsh net-destroy default`. Vérifier avec `ifconfig -a`**

  * [x] **Empêcher sa création automatique au démarrage de la vm passoire avec `virsh net-autostart default --disable`.**

### Vérifier vos connaissances sur :

- clone lié, clone intégral 
- netplan, ifupdown
- ip, ifconfig
- DHCP, dhclient


## **2. Réseau de deux VM**

Partant de la VM précédente, cet exercice permet de créer un réseau de deux VM et de réviser certaines connaissances en réseaux.

- Cloner la machine virtuelle passoire en réalisant `un clone lié`, appelé passoire-2bis".
- Lancer les deux VM passoire et passoire-2 et s'y connecter en tant qu'utilisateur etu.

### Couche 2

* **Déterminer le type de carte réseau avec la commande suivante :**

```shell
lspci | grep -i eth.
```
![image-20191001162126559](./img/image-20191001162126559.png)

- **Installer l'application lynx. A quoi sert-elle ?**

Un browser en mode text.

- **Isoler les adresses MAC de la VM à l'aide d'une commande. On pourra utiliser les commandes ifconfig, grep, tr et cut.**

```shell
ifconfig  | grep ether
>  ether   ae:35etc.   twqueuelen   1000   (ethernet)
ifconfig  | grep ether | tr -s ' ' '/' #remplacer les espaces successifs par un seul / pour le cut
>/ether/ae:35etc./twqueuelen/1000/(ethernet)
ifconfig  | grep ether | tr -s ' ' '/' | cut -d/ -f3 #d=delimiteur --> crée des champs f=field et on prend le troisième
```

- **Interroger le site hwaddress.com avec lynx et la commande précédente en argument sur le modèle : lynx hwaddress.com/?q=`ifconfig ... `. Expliquer le résultat.**

```shell
lynx hwaddress.com/?q=`ifconfig  | grep ether | tr -s ' ' '/' | cut -d/ -f3` #au lieu des ` on peut mettre $( )
```
L'adresse mac n'existe pas "No records found matching ae:35etc. " --> c'est une machine virtuelle ?

- **Est-ce qu'un contrôle d'accès réseau par adresse MAC représente une sécurité satisfaisante ?**

Non car on peut forcer l'adresse mac.

### Couche 3


* [x] **Vérifier que les deux VM ont deux adresses IP différentes.**

​		passoire-TD —— 172.23.3.20/16

​		passoire-TD-2 —— 172.23.3.21/16

* [x] **Installer le paquet ipcalc et vérifier la nature des adresses IP :**

  * apt-get install ipcalc -> sudo apt install ipcalc
  
  * ipcalc adresse_IP (il doit remplacer vrai @ip ici......) --> je pense qu'il faut plutôt remplacer adresse_IP par la vrai adresse ip
  
  passoire-TD
  
  ![image-20191002232535112](./img/image-20191002232535112.png)
  
  passoire-TD2  ![image-20191002232914366](./img/image-20191002232914366.png)

* **Ping** 

  * [x] **Réaliser un ping depuis passoire vers passoire-2. Expliquer.**

Ca marche. On est sous le même sous-réseau ?

  * [ ] **Modifier la configuration de la carte réseau dans proxmox en décochant la case "Firewall" et recommencer le ping. Expliquer.**

    ❓ why we can still connect to each other??? 

  * [ ] **Comparer les champs TTL en réalisant un ping de passoire-2, de www.utc.fr et de www.google.fr. Expliquer.**

  ```shell
  ping www.utc.fr # ttl=62
  ping www.google.fr # ttl=55
  ```

  ​		TTL —— Le TTL est une donnée placée au niveau de l'[en-tête](https://fr.wikipedia.org/wiki/Header) du [paquet](https://fr.wikipedia.org/wiki/Paquet_(réseau)) [IP](https://fr.wikipedia.org/wiki/Internet_Protocol) qui indique le nombre maximal de [routeurs](https://fr.wikipedia.org/wiki/Routeur) de transit. La valeur recommandée par le RFC 17002 est de 64. (8 bits) 

Entre passoire 1 et 2 il n'y a aucun routeur de transit donc il reste 64 d'acceptable.
Ce qui implique que entre passoire 1 et utc il y en a 2.
Entre passoire 1 et google il y en a 9. 

  * [ ] **Afficher la table de routage avec la commande route.**

  ```shell
  sudo route -n		#sans DNS
  ```

  ![image-20191003000936123](./img/image-20191003000936123.png)

### Couche 4

* [x] **Déterminer le port de telnet, ssh et http en consultant le fichier /etc/services.**

```shell
grep -E '^(ssh|telnet|http)\s' /etc/services
```

![image-20191003003425342](./img/image-20191003003425342.png)

* [x] **Dans la VM passoire-2, lancer la commande `watch -n1 'netstat -napt --ip'`. Expliquer ce qu'elle fait.**

![image-20191003003822847](./img/image-20191003003822847.png)

​	❓We should use `-napt` or `-nat` ??  le p sert à savoir le nom du programme qui utilise le port mais n'est visible que si l'on est en sudo 

​	While executing `watch -n1 'netstat -napt --ip`

​	Execute `pring all the current connected address ip` periodically every 1 s

​	https://blog.51cto.com/sadoc/1932028 (chinois...)

ça permet de voir toutes les connections aux ports réseaux (netsat -napt --ip) toute les secondes (-n1).
Ici ssh (22), telnet (23) et le dns (53) sont en écoute.

![image-20191003011155093](./img/image-20191003011155093.png)

* [x] **Depuis la VM passoire, lancer la commande `telnet` et constater l'établissement de la socket ainsi que ses différents états.** ❓

```shell
telnet <adresse_ip d'où l'on veut se connecter>
```

On remarque bien que passoire 1 s'est connectée à passoire 2 en telnet car il y a une connexion etablie ayant pour adresse locale l'ip de passoire 1 et en foreign adresse celle de passoire 2 avec pour programme in.telnetd. 


### Vérifier vos connaissances sur :

- Modèle OSI

- Couche 2

  - lspci
    
    **lspci** est une commande sous [Unix](https://fr.wikipedia.org/wiki/Unix) (et [GNU/Linux](https://fr.wikipedia.org/wiki/GNU/Linux)) qui affiche des informations très détaillées sur les [périphériques](https://fr.wikipedia.org/wiki/Périphérique_(informatique)) du [bus PCI](https://fr.wikipedia.org/wiki/Peripheral_Component_Interconnect) d'un ordinateur.
    
  - adresse MAC

#### Couche 3

* adresses IP publiques et privées, NAT

- ping,

- route

- champ TTL

  TTL —— Le TTL est une donnée placée au niveau de l'[en-tête](https://fr.wikipedia.org/wiki/Header) du [paquet](https://fr.wikipedia.org/wiki/Paquet_(réseau)) [IP](https://fr.wikipedia.org/wiki/Internet_Protocol) qui indique le nombre maximal de [routeurs](https://fr.wikipedia.org/wiki/Routeur) de transit.  (8 bits)

#### Couche 4

- - numéros de port, fichier /etc/services
  - sockets
  - netstat

- Problématiques de sécurité

- - Confidentialité des adresses
  - Usurpation des adresses -> 侵占，窃取地址
  - Découverte de réseau



## **3. Noms des machines**

Cet exercice permet de comprendre le nommage des machines.

### 1. Nom de la machine locale -> 本地主机名

* [x] **Que donnent les commandes `uname -a `et `hostname` ?**

  `uname`: print system information

![image-20191003091159161](./img/image-20191003091159161.png)

* [x] **Renommer la VM passoire-2 en passoire-2 :**

  * [x] **Utiliser la commande suivante : `hostnamectl set-hostname passoire-2`.**
  * [x] **Ajouter la ligne "127.0.01 passoire-2" au fichier /etc/hosts**

  ```shell
  sudo echo "127.0.01 passoire-2" >> /etc/hosts # permission denied ??
  ```
  ça ne marche par car la redirection est faites par le shell (de etu) avant de lancer le sudo. (le shell essaie d'ouvrir le fichier /etc/hosts en écriture et de rediriger la sortie du sudo dessus et donc avant de lancer le sudo)
  
  J'ai modifié directement le fichier avec vim.
  
  On cherche l'addresse ip d'un nom d'abord dans /etc/hosts localement dans lequel il n'y a en général que la propre ip du serveur avant de demander au dns (le fichier /etc/nsswitch.conf définit l'ordre).

  * [x] **Redémarrer la machine et vérifier le changement de nom.**

### 2. Nom des machines distantes -> 远程主机名

* [x] **Ajouter une ligne "@IP passoire-2" dans le fichier `/etc/hosts`v de la machine passoire (remplacer @IP par l'adresse IP de passoire-2).**

* [x] **Vérifier avec la commande suivante, lancée depuis passoire : `telnet passoire-2 -l etu`.**

  Il nous permet de connecter sur `passoire-TD2` avec la VM `passoire-TD`

![image-20191008141151104](./img/image-20191008141151104.png)

* How to exit telnet:

  `ctrl + ]` and then `q`
  
  ou `ctrl + d` ou `exit` pour faire arrêter le shell distant d'abord

* [x] **Faîtes de même sur passoire-2.**

* [x] **Vérifier les connexions avec `netstat -napt --ip` puis avec `netstat -apt --ip`.**

  https://linux.cn/article-2434-1.html

![image-20191008142052222](./img/image-20191008142052222.png)

| netstat |                                   |
| :-----: | :-------------------------------: |
|   -a    |             show all              |
|   -n    |           prohibit DNS            |
|   -t    |             only TCP              |
|   -p    | show info of process (using root) |
|   -u    |             only UPD              |
|   -c    |          keeping output           |
|   -l    |             listening             |

在VM-TD2上远程登录TD的账户，在该账户中使用`netstat`看到有与`10.10.10.92`（TD2的ip）的链接

### 3. Configuration locale du serveur de nom -> 本地域名服务器的配置

* [x] **Retrouver les adresses IP des machines www.utc.fr et www.google.fr avec la commande `dig`.**

  www.utc.fr

![image-20191003101443218](./img/image-20191003101443218.png)

* www.google.fr

  ![image-20191003120635603](./img/image-20191003120635603.png)

* [x] **Inversement, retrouver le nom d'une machine en partant d'une adresse IP avec la commande `dig -x`.**

  ```shell
  dig -x <@ip>
  ```

* [x] **Quel est le rôle des fichiers /etc/nsswitch.conf et /etc/resolv.conf ?**

  * /etc/hosts

    affecter DNS à la main

  * `/etc/resolv.conf` 

    qui indique quels serveurs de noms utiliser

  * `/etc/nsswitch.conf` 

    qui définit l'ordre de recherche des bases de données réseau.

  https://www.xiebruce.top/1024.html

* [x] **Retrouver le programme en charge du service avec ps aux | grep resolv et noter son numéro (pid).**

  ```shell
  ps aux | grep resolv #il faut `aux` parce que sinon il n'afficherai que les process appartenant à etu (a) et ceux associé à un terminal (x) sans afficher l'utilisateur (u)
  ps # just two processes are running??? I suppose
  #ps can't find resolv : parce que tu ne vois que les process de etu ( il faut l'option -A ou aux)
  ```

* [x] **Vérifier avec `netstat -nap --ip` (à lancer en tant qu'administrateur pour voir les programmes associés aux sockets).**

![image-20191008155352859](./img/image-20191008155352859.png)

![image-20191008155415181](./img/image-20191008155415181.png)

systemd-resolve écoute sur le port 53 en tcp et udp.

* [x] **Stopper ce processus avec kill -STOP pid et recommencer les commandes dig ci-dessus. Que constatez-vous ?**

  ```shell
  kill -STOP <PID>
  ```

  ![image-20191009100506439](./img/image-20191009100506439.png)

  `dig -x <@ip>` has been stopped
  

ça ne marche pas car il ne sait pas à qui aller demander ?

Mais il s'est remit en route tout seul ? par le process qui gère les services

* [x] **Relancer le programme avec kill -CONT pid et vérifier que les commandes dig fonctionnent.**

ça marche ..

  **`systemd-resolv`always changes PID, so it continues to run automatically when PID is changed.**

### 4. Requête au serveur de nom

* [x] **Depuis passoire-2, se connecter sur passoire en telnet avec le compte etu.**

* [x] **Lancer la commande `sudo netstat -napuc` (alternativement, on pourra utiliser watch -1 'netstat -napu'). Expliquer cette commande et son résultat.**

  un tableau comme ci-dessus

* [x] **Réaliser ensuite des requêtes dig depuis passoire et constater l'évolution de l'affichage dans la commande précédente.**

  ​	Il se passe tranquillement..... Mais le PID ne change plus.....

  ![image-20191009102836745](./img/image-20191009102836745.png)

### 5. Configuration à l'UTC

* [x] **Retrouver la configuration du serveur de nom de la machine hôte.**

  serveur de nom -> name server (ex: DNS)

  ```shell
  cat /etc/resolv.conf
  ```

  ![image-20191009104731431](./img/image-20191009104731431.png)

* [x] **Retrouver les serveurs de nom de l'UTC.**

  ![image-20191009105631011](./img/image-20191009105631011.png)

  ![image-20191009105950281](./img/image-20191009105950281.png)
  
### Vérifier vos connaissances sur :

- uname

  ![image-20191009005649095](./img/image-20191009005649095.png)

- hostname

- /etc/hostname, /etc/hosts

- dig

- /etc/resolv.conf, /etc/nsswitch.conf

  

## **4. Nouvelle interface réseau**

Dans cet exercice, on complète la vm passoire avec une nouvelle interface réseau.

* [x] **Dans la configuration de la machine virtuelle passoire (onglet Configuration/Hardware de proxmox), ajouter une interface réseau :**

  Bridge : vmbr1 (ie. valeur différente de l'interface réseau déjà présente)

  * [x] VLAN Tag : no VLAN
  * [x] Model : VirtIO
  * [x] Mac address : auto
  * [x] Firewall : ne pas cocher la case.

- [x] **Relancer passoire et récupérer le nom de l'interface réseau ainsi créée avec `dmesg | grep -i eth`.**

  ![image-20191009235516054](./img/image-20191009235516054.png)

- [x] **Editer le fichier `/etc/netplan/50-cloud-init.yaml` afin d'ajouter la configuration de cette nouvelle interface. La configuration est similaire à la première. Attention à ne pas utiliser de tabulations.**

  ![image-20191010001120896](./img/image-20191010001120896.png)

  ```shell
  sudo netplan apply
  ```

- [x] **Demander l'attribution d'une IP avec `dhclient` et vérifier avec `ifconfig` ou ip address show.**

  ```shell
  dhclient -r
  ```

  ![image-20191010002631547](./img/image-20191010002631547.png)

## **5. Sécurité des communications**

Cet exercice porte sur la confidentialité des communications et l'intérêt du chiffrement. Attention à bien respecter les consignes. Relire les articles [323-1](https://www.legifrance.gouv.fr/affichCodeArticle.do;?idArticle=LEGIARTI000030939438&cidTexte=LEGITEXT000006070719&dateTexte=20180926) et [323-3-1](https://www.legifrance.gouv.fr/affichCodeArticle.do;?idArticle=LEGIARTI000028345220&cidTexte=LEGITEXT000006070719) du code pénal au préalable.

### Capture de trafic avec tcpdump

* [x] **Sur passoire-2, lancer la commande suivante : `ping passoire-TD`.**

* [x] **Via quelle interface réseau la machine virtuelle passoire reçoit-elle les paquets ping en provenance de passoire-bis ?**

  ![image-20191010003242402](./img/image-20191010003242402.png)

* [x] **Sur passoire, lancer la commande suivante en adaptant le nom de l'interface : `sudo tcpdump -n -i interface`.**

* [x] **Expliquer la commande et les résultats obtenus.**

  ![image-20191010080830608](./img/image-20191010080830608.png)

  Même si eth1 a déjà été existant.

  Ok, fine. It should run

  ```shell
  sudo tcpdump -n -i ens19
  ```

  ![image-20191010091102185](./img/image-20191010091102185.png)
  
  ​		Résponse de `ens18`
  
  ![image-20191015160734376](./img/image-20191015160734376.png)
  
  

### Capture de contenu avec tcpdump

* [x] **Sur passoire, lancer la commande suivante : `sudo tcpdump -n -i interface 'port 23' -X`.**

  ![image-20191010081051411](./img/image-20191010081051411.png)

  It seems that we should restart networking device... So I installed `network-manager`

  ![image-20191010083030534](./img/image-20191010083030534.png)

  Ok, I'm fine :)

  Donc il doit remplacer `interface` par `ens19`

  ![image-20191015155618401](./img/image-20191015155618401.png)

* [x] **Sur `passoire-bis`, lancer la commande suivante : `telnet passoire -l etu`.**

  ⚠️: @ip de 2 VM vont être changés périodiquement.

* [x] **Expliquer les commandes et les résultats obtenus. Retrouver l'information sensible.**

  ![image-20191015160922015](./img/image-20191015160922015.png)

Après que passoire 1 ai envoyé le message contenant "Password: ", passoire 2 envoie une série de messages de longueur 1 contenant le mot de passe caractère par caractère : on peut donc le lire.

### Visualisation graphique avec Wireshark -- **pas fait**

* [ ] **Sur le système hôte, identifier les adresses IP et notamment celle le connectant à la machine virtuelle passoire.**

  @ip_host: 172.23.2.12

* [x] **Sur passoire, lancer la commande suivante :**

  **`sudo tcpdump -s0 -n -U -w - -i <interface> 'not port 3000' | nc @IP_hôte 3000`.**

  ```bash
  #dans la machine virtuel
  sudo tcpdump -i ens20 -w capture1.cap
  chmod 644 capture1.cap
  
  
  #dans la machine host
  scp etu@172.23.3.20:capture1.cap .
  wireshark capture1.cap
  ```

  会生成一个文件，通过pipeline自动转发给@ip_host的3000端口 

  -s0 -> the default length of captured traffic is 68k

  -w -> save as cap file(for wireshark)

  ```shell
  tcpdump -i eth0 -s 0 -w a.cap #infos have been saved in a.cap
  tcpdump -r a.cap #read a.cap
  ```

  -U -> each packet will be saved in the file in time

  -n -> not transform address from number to name

  ![image-20191015161431000](./img/image-20191015161431000.png)

* [ ] **Sur le système hôte, lancer ensuite la commande :**

  **`nc -k -l 3000 | wireshark -k -i -`?????❓**

  在host machine上持续监听

  -k -> 当客户端从服务端断开连接后，过一段时间服务端也会停止监听。 但通过选项 `-k` 我们可以强制服务器保持连接并继续监听端口。

  -l -> 可以进入监听模式，使我们可以在指定端口监听入站连接

* [ ] **Expliquer les différentes commandes. Que constatez-vous ?**

  En fait, dans notre VM, il n'y a pas de GUI pour tcpdump. et puis tcpdump et wireshark ils sont presque pareils. Donc ce que l'on fait c'est pour  transférer des trafics que l'on a capturé depuis VM vers notre machine réelle.

* [ ] **Observer l'encapsulation des paquets dans Wireshark et les différents champs des entêtes (adresses MAC, adresses IP, drapeaux IP, numéros de port et options TCP...).**

* [ ] **Ajouter un trafic ping entre passoire et passoire-bis.**

* [ ] **Via un clic droit sur l'un des paquets du flux TCP, afficher le contenu du flux (menu Follow TCP stream). Que constatez-vous**

### Connexion ssh

* [x] **Depuis passoire, se connecter en ssh sur passoire-bis.**

```shell
ssh etu@10.10.10.128
logout #for exiting ssh
```

* [ ] **Suivre la communication avec Wireshark. Que constatez-vous ?**

* [ ] **Corriger passoire afin d'interdire les communications non chiffrées.**

### Vérifier vos connaissances sur :

- tcpdump

  dump the traffic on the network

- nc(ncat)

  用于监听、远程登录

- wireshark

- telnet

- ssh

- confidentialité des communications



## **5. Découverte de services**

Cet exercice porte sur la détection à distance de services réseaux. Attention à bien respecter les consignes. Relire l'article [323-3-1](https://www.legifrance.gouv.fr/affichCodeArticle.do;?idArticle=LEGIARTI000028345220&cidTexte=LEGITEXT000006070719) du code pénal au préalable.

* [x] **Installer le paquet `nmap` sur passoire-2. Consulter ensuite le man de la commande nmap. Comment classer cette commande ?**

  ![image-20191021212537220](./img/image-20191021212537220.png)

### Isolation du réseau

* [x] **Afin de rester dans le réseau privé d'hôte, démonter l'interface réseau en NAT sur passoire-bis avec la commande : `sudo ip link set ens18 down`. Adapter si besoin le nom de l'interface.**

* [x] **Vérifier avec ifconfig qu'il ne reste plus que l'interface connectée au réseau vmbr1.**

* [x] **Est-il encore possible de télécharger un paquet logiciel sur la machine virtuelle ?**

  non

  ![image-20191021215130795](./img/image-20191021215130795.png)

* [ ] **Depuis passoire-2, lancer la commande suivante : nmap passoire.**
  
* [x] Erell 
  
* [ ] **A l'aide de tcpdump ou wireshark (cf. exercice précédent), observer le trafic généré par nmap.**
  * [x] Erell 
      On voit qu'il test tout les ports (plus de 900) et n'en trouve que 2 ouvert (ssh et telnet).

* [ ] **Réaliser un scan UDP sur passoire.**
  
    * [x] Erell 

```shell
etu@passoire-erell-2:~$ sudo nmap @ip-passoire-1 -sU
```
Long, pas de résultat ?


* [ ] **Réaliser une découverte d'OS sur passoire et sur l'adresse 192.168.56.1**
   * [x] Erell 

```shell
etu@passoire-erell-2:~$ sudo nmap @ip-passoire-1 -O

>Running: Linux 3.X|4.X
>OS details: Linux 3.2 - 4.8
```
Donc il dit que passoire-1 est sous Linux 3.2-4.8.
Pourtant :

```shell
etu@passoire-erell-1:~$ uname -r

>4.15.0-66-generic
```

Donc on serai en 4.15 ?

* [ ] **Pour réactiver l'interface réseau qui avait été désactiver, utiliser `sudo ip link set eth1 up`.**
   * [x] Erell 
   Faire plutôt :

```shell
etu@passoire-erell-1:~$ sudo ip link set ens18 up
```
  non ?


### Vérifier vos connaissances sur :

- nmap
- scan de port



## **6. Segmentation des privilèges** 

Cet exercice porte sur les comptes de service et la séparation des privilèges.

### Comptes de service

* [x] **Compter le nombre d'utilisateurs définis dans le fichier /etc/passwd. Expliquer.**

  ```shell
  wc -l /etc/passwd
  ```

  ![image-20191021233814014](./img/image-20191021233814014.png)

 ```shell
 cut -d: -f2,1,7 /etc/passwd | sort -n -t: -k2n #pour trier les utilisateur selon leur id
 ```

On remarque qu'il n'y que 4 utilisateurs dans les 1000 qui sont de 'vrais' utilisateurs.

Les utilisateurs ayant des id plus petits sont probablement utiliser pour des demon ou des services du système. La plupart n'ont pas de vrai shell (`usr/sbin/nologin` ou `/bin/false` ) les seuls a avoir un vrai shell sont (`grep -v nologin /etc/passwd | grep -v false`) root, sync?, www-data, et les quatres sités plus haut.

Les deux autres (
`libvirt-qemu:64055` 
et 
`nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin`
) je ne sais pas.

* [x] **Que font les commandes `nologin` et `false` ?**

  * **nologin**

  ![image-20191021233943859](./img/image-20191021233943859.png)

  * false

    ![image-20191021234041956](./img/image-20191021234041956.png)

  * When `/sbin/nologin` is set as the shell, if user with that shell logs in, they'll get a polite message saying 'This account is currently not available.' This message can be changed with the file `/etc/nologin.txt`.

    `/bin/false` is just a binary that immediately exits, returning false, when it's called, so when someone who has `false` as shell logs in, they're immediately logged out when `false` exits. Setting the shell to `/bin/true` has the same effect of not allowing someone to log in but `false` is probably used as a convention over `true` since it's much better at conveying the concept that person doesn't have a shell.

* [x] **Expliquer les commandes suivantes et leur résultat.**

  ```bash
  grep -v home /etc/passwd | grep bash
  #find the user who has bash but don't has home
  grep -v nologin /etc/passwd | grep -v false
  #find users who can login successfully
  ```

  ![image-20191021234429621](./img/image-20191021234429621.png)

  ![image-20191021234549290](./img/image-20191021234549290.png)

* [ ] **Corriger l'anomalie trouvée.**

  Supprimer tournesol ?

### Droit spécifique pour relancer Apache

* [x] **Que fait la commande `apache2ctl` ?**

  ![image-20191022000631580](./img/image-20191022000631580.png)

  ```bash
  sudo apt-get install apache2
  sudo apache2ctl start #it should add sudo
  ```

  ![image-20191022224301522](./img/image-20191022224301522.png)

  First time start apache failed

  ```bash
  sudo netstat -tulpn | grep : 80
  ```

  ![image-20191022224501215](./img/image-20191022224501215.png)

  ```bash
  fuser -k -n tcp 80 #kill all the processes running at port 80
  sudo systemctl status apache2 #watching apache server's status
  sudo service apache2 stop/start
  ```

  ```bash
  sudo a2dissite #to disenable some sites in /apache2/sites-available
  sudo service apache2 restart #reload apache2 config
  sudo vim /etc/capache2/sites-available/mysite.conf
  ```

  ```html
  <VirtualHost *:80> -> declare that it's a virtual host and it will occupy port 80
  
  		ServerName www.mysite.com -> 域名
  
  		ServerAdmin <mail_address>
  
  		DocumentRoot /var/www/mysite ->本地网站的目录
  
  		ErrorLog ${APACHE_LOG_DIR}/mysite_error.log
  
  		CustomLog ${APACHE_LOG_DIR}/mysite_access.log combined
  
  </VirtualHost>
  ```

  ```bash
  sudo mkdir /var/www/mysite #build the main dir for site
  sudo chown etu:etu -R /var/www/mysite
  cd /var/www/mysite
  sudo vim index.html
  sudo a2ensite
  ```

* [x] **Vérifier que les utilisateurs `remus` et `romulus` ne peuvent relancer le serveur web de passoire.**

  ![image-20191023150254662](./img/image-20191023150254662.png)

* [x] **Ouvrir un autre terminal et lancer la commande suivante pour constater les essais infructueux de remus et romulus : `sudo tail -f /var/log/auth.log`.**

  ❌ ça marche pas 
  Moi ça marche (lance le en taâche de fond pour le voir bien `sudo tail -f /var/log/auth.log &` et retest avec Remus par exemple).

* [x] **En tant qu'utilisateur etu, éditer le fichier /etc/sudoers avec la commande : `sudo vi /etc/sudoers`.**

  * [x] **Dans la section *User alias specification*, ajouter la ligne : `User_Alias ROMAINS=remus,romulus`**
  * [x] **Dans la section *Cmnd alias specification,* ajouter la ligne : `Cmnd_Alias APACHE=/usr/sbin/apache2ctl`**
  * [x] **A la fin de la section *User privilege specification*, ajouter la ligne :` ROMAINS ALL=APACHE`**

* [x] **Vérifier que les utilisateurs remus et romulus peuvent maintenant relancer le site web.**

  ![image-20191023151042989](./img/image-20191023151042989.png)

  Sur `remus` je peux relancer apache2, mais je peux utiliser ni `service` , ni `systemctl`, mais `apache2ctl` marche bien.

  ![image-20191023151344052](./img/image-20191023151344052.png)

### Droit spécifique pour éditer le site web

* [x] **A qui appartient la page web par défaut du site web de passoire ?**

![image-20191023152901295](./img/image-20191023152901295.png)

​		C'est root

* [x] **En tant qu'utilisateur etu, changer récursivement le user et le group du répertoire `/var/www/html` de sorte qu'il appartienne à `www-data`.**

  ```bash
  sudo chown www-data:www-data -R /var/www/html
  ```

  ![image-20191023153756177](./img/image-20191023153756177.png)

* [x] **Vérifier que l'utilisateur etu peut éditer le fichier index.html sans modifier ses droits avec la commande : `sudo -u www-data vi /var/www/html/index.html`.**

  ![image-20191023153951063](./img/image-20191023153951063.png)

  Oui, je peux le faire.

* [x] **Modifier ce fichier pour qu'il affiche simplement "Bienvenue" aux visiteurs. Vérifier depuis passoire-2 avec la commande `lynx`.**

  Sur passoire-TD2, lance `curl 10.10.10.106:80` failed

  ![image-20191023161111342](./img/image-20191023161111342.png)

  .....It changed @ip secretly -> I add new interfaces Internet

  And then lance `lynx 172.23.3.20:80` success

  ![image-20191023161622025](./img/image-20191023161622025.png)

  Sur passoire, lance `lynx http://127.0.0.1` -> success

  ![image-20191023161416614](./img/image-20191023161416614.png)

* [x] **Ajouter un utilisateur `cesar` et faire en sorte qu'il puisse éditer le fichier index.html de deux manières différentes :**

  * [x] **En modifiant le fichier sudo**
  
    ![image-20191023171300109](./img/image-20191023171300109.png)
  
    ```bash
    sudo adduser cesar sudo # DANGER cesar a tout les droits sur tout
    sudo -u www-data vi /var/www/html/index.html
    ```

    ```bash
    sudo visudo
    # Ajout dans le document de :

    cesar ALL= (www-data) /usr/bin/vi /var/www/html/index.html
    cesar@passoire-erell-1:~$ sudo -u www-data vi /var/www/html/index.html
    ```
  
  * [x] **En utilisant les groupes.**
  
    ```bash
    sudo gpasswd -a cesar www-data
    # j'ai fait : sudo adduser cesar www-data
    sudo chmod 660 /var/www/html/index.html
    ```

### Vérifier vos connaissances sur :

- La configuration des comptes de service
- La commande sudo et le fichier /etc/sudoers
- La page par défaut du serveur web Apache

## **7. Isolation d'un utilisateur** 

Cet exercice porte sur l'isolation d'un utilisateur à l'aide de chroot. La même technique peut être appliquée pour isoler un service.

### Construction de l'isolation minimale
* [x] Erell
  * [x] **Créer le répertoire `/var/isoler` qui servira de "cage" pour notre isolation. Ajouter le répertoire `/var/isoler/bin`.**
```bash
sudo mkdir -p /var/isoler/bin
```
  * [x] **Copier /bin/bash dans /var/isoler/bin.**
```bash
sudo cp /bin/bash /var/isoler/bin
```
  * [x] **Rechercher les bibliothèques partagées utilisées par bash avec la commande suivante : `ldd /bin/bash`.**

    ![image-20191026235655854](./img/image-20191026235655854.png)

      * [x] **Copier ces bibliothèques dans la cage ; créer pour cela les répertoires `/var/isoler/lib`, `/var/isoler/lib64`, `/var/isoler/lib/x86_64-linux-gnu`. On veillera à ne copier que les bibliothèques nécessaires à l'exécution de bash.**

        ```bash
        sudo mkdir /var/isoler/lib
        sudo mkdir /var/isoler/lib/x86_64-linux-gnu
        sudo mkdir /var/isoler/lib64
        sudo cp /lib/x86_64-linux-gnu/libtinfo.so.5 /var/isoler/lib/x86_64-linux-gnu
        ...
        sudo cp /lib64/ld-linux-x86-64.so.2 /var/isoler/lib64
        ```

        

### Utilisation de la cage construite
* [x] Erell
* [x] **Lancer l'environnement isolé avec la commande suivante : `chroot /var/isoler`.**
  `chroot` = ça change la racine de fichier, ça cache et protège tout le reste car on ne peut accéder à aucun fichier en dehors de cet environement.

  ![image-20191027001737692](./img/image-20191027001737692.png)

* [x] **Exécuter les commandes suivantes : pwd, echo *, cd bin, echo *, pwd**

  * pwd

    ![image-20191027001830596](./img/image-20191027001830596.png)

  * echo *

    ![image-20191027001905539](./img/image-20191027001905539.png)

  * cd bin, echo *

    ![image-20191027001942986](./img/image-20191027001942986.png)

  * pwd

    ![image-20191027001959201](./img/image-20191027001959201.png)

* [x] Pourquoi ces commandes sont acceptées ?
  Car ce sont des shell builtin (`type pwd`) i.e. gérés directement par bash (et vu que dans la cage le seul excutable est bash d'autres type de commandes ne marcheraient pas cf. 4. ).

* [x] Essayer d'autres commandes telles que ls, vi, df...

  ![image-20191027003634415](./img/image-20191027003634415.png)

```bash
type ls
>/bin/ls
type vi
>usr/bin/vi
type df
>/bin/df
```

### Compléments
* [x] Erell
* [ ] Ajout des commandes `ps` et `ls` avec la même méthode que pour bash ci-dessus :
* [ ] Copie des exécutables.
* [ ] Recherche des bibliothèques partagées nécessaires.
* [ ] Copie des bibliothèques partagées.

```shell
ldd /bin/ps | grep \> | cut -d\> -f2 | while read l unused; do printf "sudo cp %s /var/isoler%s\n" "$l" "$l"; done | bash
```
4. Vérifier au sein de l'environnement isolé.
Ps ne marche pas ?
`Error, do this: mount -t proc proc /proc`

   3. Ajout du périphérique spécial null :

   4. 1. Créer le répertoire pour les *devices* : mkdir /var/isoler/dev
      2. Créer le périphérique null : mknod /var/isoler/dev/null c 1 3
      3. Vérifier avec la commande suivante dans l'environnement isolé : ls bin toto 2> /dev/null
      En gros on crée le trou noir.

   5. Ajout du pseudo système de fichier /proc qui contient des informations sur (tout) le système en exécution :

   6. 1. Créer le point de montage : mkdir /var/isoler/proc
      2. Monter le pseudo système de fichier une seconde fois (il l'est déjà dans /proc) : `mount --bind /proc /var/isoler/proc`
Monter = donner un point dans l'arborescence du fichier qui permet d'accéder à un autre disque ou système de fichier "spécial". (ex. clé USB n'est pas naturellement dans / sauf si on la Monte en un point)

Ici : `/var/isoler/proc` va pointer au même endroit que `/proc` (qui pointe vers le système de fichiers spécial `proc`)

```shell
mount | grep proc
>proc on /proc type proc
```
      3. Au sein de l'environnement isolé, examiner les informations relatives aux processus auxquelles /var/isoler/proc donne accès.

### Création d'un utilisateur ayant un shell isolé
* [x] Erell
1. 1. Ecrire un shell-script /usr/local/bin/isoler.sh contenant les lignes suivantes :

   2. 1. \#!/bin/bash
      2. echo "Bienvenue dans ce shell isolé par chroot"
      3. /usr/sbin/chroot /var/isoler/ /bin/bash

   3. Ajouter les droits d'exécution à ce script.

   4. Ajouter l'utilisateur de login *minus* qui aura pour *home directory* le répertoire /var/isoler et pour shell /usr/local/bin/isoler.sh via la commande suivante : `useradd -c minus -d /var/isoler -s /usr/local/bin/isoler.sh`
`useradd -c minus -d /var/isoler -s /usr/local/bin/isoler.sh minus` je pense plutôt

   5. Attribuer un mot de passe à minus avec la commande passwd.

   6. Ajouter le setuid bit à chroot.

   7. Tester en se connectant en tant que utilisateur minus.
Il faut donner les droit a+rx à /usr/local/bin/isoler.sh et a+x à /var/isoler pour que ça marche.