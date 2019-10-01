#  TD1

* create new user and home directory

  ```shell
  sudo adduser <username>
  ```

* regarder le mot de pass de tous des utilisateurs

  ```shell
  cat /etc/group
  ```

* change current user

  ```shell
  su <username>
  ```

* save a new file in vim

  ```shell
  :file <filename>
  :wq
  ```

**whoami** -- display effective user id

**users** -- list current users

**ls -l**

![image-20190923152322589](/Users/haida/Library/Application Support/typora-user-images/image-20190923152322589.png)



* Create a new group with existing users

	```shell
	sudo groupadd <groupname>
	sudo gpasswd <username> <groupname>
	```

* Change file owner and group

  ```shell
  chown remus:rome update.sh
  ```

  组内权限的范围为文件所有主所在的组，所以可能需要移交所有主

* Change group ownership

  ```shell
  chgrp <groupname> <filename>
  ```

  

# TD2

Déterminer le type de carte réseau avec la commande suivante :

```shell
 lspci | grep -i eth
```

