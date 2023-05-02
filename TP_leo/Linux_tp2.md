TP2 linux
--- 
---
---

I.Service SSH
1. Analyse du service

🌞 S'assurer que le service sshd est démarré

````
[bossautruche@localhost ~]$ systemctl status sshd
● sshd.service - OpenSSH server daemon
     Loaded: loaded (/usr/lib/systemd/system/sshd.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2023-02-09 15:11:46 CET; 3min 17s ago
`````
🌞 Analyser les processus liés au service SSH
````
[bossautruche@localhost ~]$ systemctl status | grep sshd
           │ ├─sshd.service
           │ │ └─687 "sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups"
               │ ├─856 "sshd: bossautruche [priv]"
               │ ├─860 "sshd: bossautruche@pts/0"
               │ └─894 grep --color=auto sshd
````
🌞 Déterminer le port sur lequel écoute le service SSH

````
[bossautruche@localhost ~]$ ss -n | grep 10.3.2
tcp   ESTAB 0      52                      10.3.2.254:22        10.3.2.1:58194
````
🌞 Consulter les logs du service SSH

````
[bossautruche@localhost ~]$ journalctl | grep ssh
Feb 09 15:11:44 localhost systemd[1]: Created slice Slice /system/sshd-keygen.
Feb 09 15:11:45 localhost systemd[1]: Reached target sshd-keygen.target.
Feb 09 15:11:46 localhost sshd[687]: Server listening on 0.0.0.0 port 22.
Feb 09 15:11:46 localhost sshd[687]: Server listening on :: port 22.
Feb 09 15:12:56 localhost.localdomain sshd[856]: Accepted password for bossautruche from 10.3.2.1 port 58194 ssh2
Feb 09 15:12:56 localhost.localdomain sshd[856]: pam_unix(sshd:session): session opened for user bossautruche(uid=1000) by (uid=0)
Feb 09 17:21:28 localhost.localdomain sshd[987]: Accepted password for bossautruche from 10.3.2.1 port 49404 ssh2
Feb 09 17:21:28 localhost.localdomain sshd[987]: pam_unix(sshd:session): session opened for user bossautruche(uid=1000) by (uid=0)
````
-------------------------------------------------------------
````
[bossautruche@localhost /]$ cd var/log
[bossautruche@localhost log]$ ls
anaconda       chrony           dnf.log      hawkey.log-20230209  messages           secure            sssd
audit          cron             dnf.rpm.log  lastlog              messages-20230209  secure-20230209   tallylog
btmp           cron-20230209    firewalld    maillog              private            spooler           wtmp
btmp-20230209  dnf.librepo.log  hawkey.log   maillog-20230209     README             spooler-20230209
````
 🌞 Identifier le fichier de configuration du serveur SSH

on se fait pas chier :

````
[bossautruche@localhost /]$ sudo find /etc/ssh/ -name "*config*"
[sudo] password for bossautruche:
/etc/ssh/ssh_config.d
/etc/ssh/sshd_config.d
/etc/ssh/sshd_config
/etc/ssh/ssh_config
````

🌞 Modifier le fichier de conf

````
[bossautruche@localhost ssh]$ sudo nano sshd_config
[bossautruche@localhost ssh]$ sudo cat sshd_config | grep 5619
#Port 5619
````
j'ai mis un port au hasard moi même car je suis un enfant independant et je n'ai pas besoin qu'un shell choisisse un numero a ma place, et toc !

````
[bossautruche@localhost ssh]$ sudo firewall-cmd --add-port=5619/tcp --permanent
success
[bossautruche@localhost ssh]$ sudo firewall-cmd --remove-port=22/tcp --permanent
Warning: NOT_ENABLED: 22:tcp
success
[bossautruche@localhost ssh]$ sudo firewall-cmd --reload
success
[bossautruche@localhost ssh]$ sudo firewall-cmd --list-all | grep 5619
  ports: 5619/tcp
  ````

🌞 Redémarrer le service
  ````
  [bossautruche@localhost ssh]$ sudo systemctl restart sshd
  ````

🌞 Effectuer une connexion SSH sur le nouveau port
````
PS C:\Users\pc> ssh bossautruche@10.3.2.254 -p 5619
bossautruche@10.3.2.254's password:
````

I. Service HTTP
---
1. Mise en place


🌞 Installer le serveur NGINX

````
sudo dnf upgrade
````

histoire d'upgrade tous les paquets ça fait pas de mal

````
sudo dnf install nginx -y
````

🌞 Démarrer le service NGINX

````
 sudo systemctl start nginx
 ````

 🌞 Déterminer sur quel port tourne NGINX
````
  sudo ss -alnpt | grep nginx
  ````

  🌞 Déterminer le path du fichier de configuration de NGINX

  ````
  which nginx
  ````
  permet de trouver nginx
  








