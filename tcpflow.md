Utilisation de tcpflow

* [x] Installer tcpflow

* [x] Créer un répertoire spécifique, eg. FLOWS.

* [x] Appliquer tcpflow au fichier newdate3.log de sorte que les flux TCP soient stockés dans le répertoire FLOWS.

  ```bash
  sudo tcpflow -i ens33 -o ./FLOWS -r newdat3.pcap 
  ```

  

* [x] Examiner les flux obtenus. Utiliser la commande "file" pour déterminer leurs types. Cette commande peut-elle se tromper quant à la nature de certains fichiers ?

  ```bash
   ls | sed "s:^:`pwd`/:" | xargs file
   #report.xml
  ```

* [x] En utilisant le shell, afficher le contenu des fichiers de flux qui sont de type texte. Indice : utiliser for, file, grep, cut, xargs

  ```bash
  ls | sed "s:^:`pwd`/:" | xargs file | grep -v xml | grep text | cut -d ':' -f 1 | xargs cat
  ```

* [ ] Analyser ce contenu.

* [ ] A votre avis, que contiennent les flux de type fichiers compressés ? (indice : se référer aux sessions ftp découvertes avec la commande précédente).

* [ ] Explorer ces fichiers compressés. Attention, il s'agit de (vieux) outils d'attaques et vous ne devriez pas les conserver après ce TD. Comment appelle-t-on ce type d'outils ?