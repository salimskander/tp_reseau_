🌞 Déterminez, pour ces 5 applications, si c'est du TCP ou de l'UDP

🦈 League_Of_Legend Ici le jeu utilise le UDP

IP : 162.249.73.149 PORT : 5189 PORT SOURCE : 52842

 Spotify : TCP

PS C:\Users\pc> netstat ; -n -b
Connexions actives
  Proto  Adresse locale         Adresse distante       État
  TCP    10.33.17.115:62306      104.199.65.124:4070    ESTABLISHED


PS C:\Users\pc> netstat -n -b
Connexions actives
  Proto  Adresse locale         Adresse distante       État
  TCP    10.33.17.115:53231      199.232.58.249:443     ESTABLISHED


  TCP    10.33.17.115:14654      52.95.126.138:443    ESTABLISHED
 Netflix

🌞 Demandez l'avis à votre OS

votre OS est responsable de l'ouverture des ports, et de placer un programme en "écoute" sur un port
il est aussi responsable de l'ouverture d'un port quand une application demande à se connecter à distance vers un serveur
bref il voit tout quoi
utilisez la commande adaptée à votre OS pour repérer, dans la liste de toutes les connexions réseau établies, la connexion que vous voyez dans Wireshark, pour chacune des 5 applications Il faudra ajouter des options adaptées aux commandes pour y voir clair. Pour rappel, vous cherchez des connexions TCP ou UDP.
# MacOS
$ netstat
# GNU/Linux
$ ss
# Windows
$ netstat