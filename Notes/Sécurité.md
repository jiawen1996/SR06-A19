目的是为了获得一个最大performant的网络

# Sécurisation des réseaux

## Différents types

MAN: 不同区域

UTC买了一个fibre

UTC我们有一个opérateur来和巴黎的几个点interconnecter

## Cartographie du réseau et du système d’information

是谁给你的权利进网？

démarche:PSSI

CMDB: les matériel, des configurations

github: 可以储存des configs complètes 

plusieurs visions - pour assurer les cours 需要访问权限

vision application/fonctionnelle 用来配置

从métier的层面如果inspect de métier觉得需要在technique层进行一些措施，就会

主要目的是cloisonner

如果datacenter principal 倒下了，还是可以访问网站 - 但不是backup❓

## Protection physique

### Protection de l'accès physique aux locaux

concentrer des 

switch是否每个人可以访问，要protéger 访问权限

如果没有supervision de réseau，访问权限很难管理

### Protection de l’accès physiques aux équipements

有disque dûr来管理我们的

但是如果这个硬盘被偷了，我们也可以防止信息

AP也有一个port console par défault，所以我们要用mot de passe 来保护他，如果进入了就可以获得整个campus的网络

用supervision来监控温度来保护

### Protection logique

两个防火墙是可以的，还可以放一个DMZ，尝试着放maximum de pare-feu

gérer des filtages qu'on va mise en place

* Isolation des machines non normalisées

  我们会isoler windows7

* BYOD

* 不允许任何的passe-droit

* 即使更新病毒软件

* 管理更新操作系统和软件

  * CERT
  * 不要有多余的软件

* filter 

* 管理个人和设备的认证和授权

  * 使用802.1x
  * EAP

* Sécuriser tous les réseaux sans-fils

  * 至少使用WPA2-Entreprise

* Utilisation systématique d’un serveur authentification , d’autorisation et d’accounting (AAA) :

  * 目的是为了管理证书，我们需要有一个

  * 一直有一个认证的服务器，用来询问LDAP或者PKI，这个人可不可以访问
  * token的好处是：onetime passwd - 

* Accès distant:

  * DMZ accès distant

    有不同的solutions针对不同的populations

* 要有不同的privilage

  如果有一个技工要来修AP，admin应该给他什么权限

### Supervision

我们会log所有的événements

给那个MAC配来哪个ip地址

我们

### IDS

des signatures

ids不会直接couper l'interface没有actions

### Prévention d'intrusion sur le réseau

### Prévention d'intrusion sur les serveurs

除了假设一个复杂的DMZ之外，还要有一个端口来启动firewall

### Pots de miel

### Exemple de pare-feu statefull sur machine Linux : IPtables

规则链

如果有一个包进来了，就会开始从规则链的顶端一个一个询问是否符合parcourir

知道遇到一个regle,match

log很重要 - pour debug

mark: traitement différent au niveau du routage

return: 规则链是hiérachie的，如果一个包被拒绝了，会交给下一个规则链进行analyse

#### Chaînes prédéfinies dans Iptables :

如果我在一台machine上安装了

包会entrer par la chaine INPUT

2 chaines sont analysés

![image-20191219122258168](/Users/haida/Library/Application Support/typora-user-images/image-20191219122258168.png)

notion de chaines séquentiel

Table filter-> 

Table NAT -> gérer 流量分析来翻译网址

MANGLE-> TOS 定义了服务质量QoS，passe的时候在哪一个priorité

#### Cheminement complet à travers les tables et chaînes :

我们需要知道我们在哪一层上被bloquée了

第一件要做的事：changer le politique 更改默认filter de la chaine INPUT

```bash
iptables -A INPUT -i eth0 -p tcp --dport 22 –j ACCEPT
##la porte destination -j: ce que je fait sur ce paquet
```



-m state -> 使用模型



#### ftp

用21端口建立连接

传输文件在20端口上