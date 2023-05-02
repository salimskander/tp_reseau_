Partie 1 : Mise en place et maîtrise du serveur Web

1. Installation
🌞 Installer le serveur Apache

[bossautruche@web ~]$ sudo dnf install -y httpd


🌞 Démarrer le service Apache

```
[bossautruche@web ~]$ sudo systemctl start httpd
```
pour start le serv

```
[bossautruche@web ~]$ sudo systemctl status httpd
```
check si tout se passe bien
```
[bossautruche@web ~]$ sudo ss -alpnt | grep httpd
```
on filtre pour voir sur quel port ca ecoute
```
[bossautruche@web ~]$ sudo systemctl enable httpd
```
on fait en sorte que le serv demarre directement au demarrage du systeme


🌞 TEST

```
[bossautruche@web ~]$ sudo systemctl is-active httpd
active
```
on check si il est en cours d'execution
```
[bossautruche@web ~]$ sudo systemctl is-enabled httpd
enabled
```
enabled = il demarre tout seul au demarrage du systeme
```
[bossautruche@web ~]$ curl localhost | head
```
check si ca fonctionne


2. Avancer vers la maîtrise du service
🌞 Le service Apache...

```
[bossautruche@web ~]$ cat /usr/lib/systemd/system/httpd.service
```
cat pour afficher le contenue du fichier


🌞 Déterminer sous quel utilisateur tourne le processus Apache

```
[bossautruche@web ~]$ cat /etc/httpd/conf/httpd.conf | grep apache
User apache
```
grep pour afficher ce qui nous interesse
```
[bossautruche@web ~]$ ls -al /usr/share/testpage/
total 12
drwxr-xr-x.  2 root root   18 Avr 17 18:19 .
drwxr-xr-x. 91 root root 4096 Avr 17 18:19 ..
```

🌞 Changer l'utilisateur utilisé par Apache

```
[bossautruche@web ~]$ sudo useradd boss -d /usr/share/httpd/ -s /sbin/nologin -u 2000
```
new user qui s'appelle boss

-----

je n'ai pas reussi cette partie
--------------
j'ai demandé de l'aide mais je n'ai toujours pas compris parfaitement
```
[bossautruche@localhost ~]$ sudo cat /etc/passwd | tail -2
apache❌48:48:Apache:/usr/share/httpd:/sbin/nologin
lucas❌6969:6970::/usr/share/httpd:/sbin/nologin
[bossautruche@localhost ~]$ sudo nano /etc/httpd/conf/httpd.conf
[bossautruche@localhost ~]$ sudo cat /etc/httpd/conf/httpd.conf | grep lucas
User lucas
Group lucas
[bossautruche@localhost ~]$ sudo systemctl restart httpd
[bossautruche@localhost ~]$ systemctl status httpd
● httpd.service - The Apache HTTP Server
     Loaded: loaded (/usr/lib/systemd/system/httpd.service; enabled; ve>     Active: active (running) since Mon 2023-01-09 09:16:54 CET; 33s ago       Docs: man:httpd.service(8)
   Main PID: 1487 (httpd)
     Status: "Total requests: 0; Idle/Busy workers 100/0;Requests/sec: >      Tasks: 213 (limit: 4631)
     Memory: 40.8M
        CPU: 80ms
[bossautruche@localhost ~]$ ps -ef | grep lucas
lucas       1488    1487  0 09:16 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
lucas       1489    1487  0 09:16 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
lucas       1490    1487  0 09:16 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
lucas       1491    1487  0 09:16 ?        00:00:00 /usr/sbin/httpd -DFOREGROUND
elbatis+    1706    1410  0 09:17 pts/0    00:00:00 grep --color=auto lucas
```
j'ai compris qu'il fallait modifier le fichier de conf et faire en sorte que "boss" deviennent l'utilisateur par defaut.
mais a chaque fois que je restart apache, rien ne change.

🌞 Faites en sorte que Apache tourne sur un autre port

```
[bossautruche@web ~]$ sudo nano /etc/httpd/conf/httpd.conf
[bossautruche@web ~]$ cat /etc/httpd/conf/httpd.conf | grep Listen
Listen 8080
[bossautruche@web ~]$ sudo firewall-cmd --remove-port=80/tcp --permanent
success
[bossautruche@web ~]$ sudo firewall-cmd --add-port=8080/tcp --permanent
success
[bossautruche@web ~]$ sudo firewall-cmd --reload
success
[bossautruche@web ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 8080/tcp

  ```

  Partie 2
  -------

🌞 Install de MariaDB sur db.tp5.linux

```
[bossautruche@db ~]$ sudo dnf install mariadb-server
[bossautruche@db ~]$ sudo systemctl enable mariadb
[bossautruche@db ~]$ sudo systemctl start mariadb
[bossautruche@db ~]$ sudo mysql_secure_installation
```
```
[bossautruche@db ~]$ sudo systemctl is-enabled mariadb
enabled
``` 

🌞 Port utilisé par MariaDB
```
[bossautruche@db ~]$ sudo ss -tlnp | grep mysql
LISTEN 0      80                 *:3306            *:*    users:(("mysqld",pid=26321,fd=21))
```

```
[bossautruche@db ~]$ sudo firewall-cmd --add-port=3306/tcp --permanent
success
[bossautruche@db ~]$ sudo firewall-cmd --reload
success
```
🌞 Processus liés à MariaDB

[bossautruche@db ~]$ ps -ef | grep mysqld | grep usr
mysql      26321       1  0 09:49 ?        00:00:01 /usr/libexec/mysqld --basedir=/usr


Partie 3 
---
 🌞 Préparation de la base pour NextCloud

[bossautruche@db ~]$ sudo mysql -u root -p

🌞 Exploration de la base de données

[bossautruche@web ~]$ sudo dnf install mysql -y


[bossautruche@web ~]$ mysql -u root -p

2.Serveur Web et NextCloud
🌞 Install de PHP

```
[bossautruche@web ~]$ sudo dnf check-update
[bossautruche@web ~]$ sudo dnf update
[bossautruche@web ~]$ sudo dnf install dnf-utils
[bossautruche@web ~]$ sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm
[bossautruche@web ~]$ sudo dnf install https://rpms.remirepo.net/enterprise/remi-release-8.rpm
[bossautruche@web ~]$ sudo dnf module reset php
[bossautruche@web ~]$ sudo dnf install php
[bossautruche@web ~]$ php -v
```

🌞 Install de tous les modules PHP nécessaires pour NextCloud

```
[bossautruche@web ~]$ sudo dnf install -y libxml2 openssl php81-php php81-php-ctype php81-php-curl php81-php-gd php81-php-iconv php81-php-json php81-php-libxml php81-php-mbstring php81-php-openssl php81-php-posix php81-php-session php81-php-xml php81-php-zip php81-php-zlib php81-php-pdo php81-php-mysqlnd php81-php-intl php81-php-bcmath php81-php-gmp
[...]
Complete!
```

🌞 Récupérer NextCloud

```
[bossautruche@web ~]$ sudo mkdir /var/www/tp5_nextcloud -p
[bossautruche@web ~]$ curl -SLO https://download.nextcloud.com/server/prereleases/nextcloud-25.0.0rc3.zip
[bossautruche@web ~]$ unzip nextcloud-25.0.0rc3.zip
[bossautruche@web tp5_nextcloud]$ sudo mv nextcloud/* /var/www/tp5_nextcloud/
[bossautruche@web tp5_nextcloud]$ ls -al | grep index.html
-rw-r--r--.  1 bossautruche bossautruche   156 Oct  6 14:42 index.html
[bossautruche@web ~]$ sudo chown -R apache:apache /var/www/tp5_nextcloud/
[bossautruche@web www]$ ls -al | grep tp5_nextcloud
drwxr-xr-x. 14 apache apache 4096 Jan  9 12:03 tp5_nextcloud
```
🌞 Adapter la configuration d'Apache

```
[bossautruche@web ~]$ sudo cat /etc/httpd/conf/httpd.conf | tail -1
IncludeOptional conf.d/*.conf
[bossautruche@web conf.d]$ sudo touch website.conf
[bossautruche@web conf.d]$ sudo cat website.conf
<VirtualHost *:80>
  # on indique le chemin de notre webroot
  DocumentRoot /var/www/tp5_nextcloud/
  # on précise le nom que saisissent les clients pour accéder au service
  ServerName  web.tp5.linux

  # on définit des règles d'accès sur notre webroot
  <Directory /var/www/tp5_nextcloud/>
    Require all granted
    AllowOverride All
    Options FollowSymLinks MultiViews
    <IfModule mod_dav.c>
      Dav off
    </IfModule>
  </Directory>
</VirtualHost>
```

🌞 Redémarrer le service Apache

[bossautruche@web conf.d]$ sudo systemctl restart httpd

3 Finaliser l'installation de NextCloud

🌞 Exploration de la base de données
















