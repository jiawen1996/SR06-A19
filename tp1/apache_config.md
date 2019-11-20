# Apache

网页的后台一般是交给apache处理的

```bash
nmap 127.0.0.1 #查看本地有哪些端口已经开出来了
curl http://127.0.0.1 #查看本地80端口是否生效
```

## 如何设置Apache的文档目录

`/etc/apache2` ->

`/etc/apache2/sites-available` -> 本地有哪些网站可以使用

```bash
sudo a2dissite #禁用一些网站
sudo service apache2 restart #刷新apache配置
sudo vim /etc/apache2/sites-available/mysite.conf
```

```html
<VirtualHost *:80> -> 说明这是个虚拟主机，占用80端口

​		ServerName www.mysite.com -> 域名

​		ServerAdmin <mail_address>

​		DocumentRoot /var/www/mysite ->本地网站的目录

​		ErrorLog ${APACHE_LOG_DIR}/mysite_error.log

​		CustomLog ${APACHE_LOG_DIR}/mysite_access.log combined

</VirtualHost>
```

```bash
sudo mkdir /var/www/mysite #建立网站主目录
sudo chown etu:etu -R /var/www/mysite
cd /var/www/mysite
vim index.html
sudo a2ensite
```

