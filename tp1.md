Tp 1 reseau Godefroy

Exploration Solo
----------------

🌞 Affichez les infos des cartes réseau de votre PC
    #commandes utilisées
       
        ipconfig /all
        

Carte réseau sans fil Wi-Fi, 

28-16-AD-2E-C7-19, 

10.33.17.31

Carte ethernet ethernet,

40-B0-34-F0-E5-0E

IP : pas connecté

🌞 Affichez votre gateway

        ipconfig /all :
 ip : 10.33.19.254
        
🌞 Déterminer la MAC de la passerelle
        
        arp -a
 MAC : 00-c0-e7-e0-04-4e

🌞 Trouvez comment afficher les informations sur une carte IP (change selon l'OS)

- panneau de config > reseau et internet > centre reseau et partage > wifi ynov > details

and voila..

🌞 Utilisez l'interface graphique de votre OS pour changer d'adresse IP : 

- panneau de config > reseau et internet > centre de reseau et partage > modifier les parametres de la carte > wifi > proprietés > ipv4



🌞 Il est possible que vous perdiez l'accès internet. Que ce soit le cas ou non, expliquez pourquoi c'est possible de perdre son accès internet en faisant cette opération.

- si l'adresse ip existe deja, on perd la prio on ne recoit plus de données reseau (donc pas d'internet)
  

Exploration Duo
---------------
🌞 Modifiez l'IP des deux machines pour qu'elles soient dans le même réseau

adresse ip changée sur les deux pc : 10.10.10.225

masque changée sur les deux pc :     255.255.255.0


🌞 Vérifier à l'aide d'une commande que votre IP a bien été changée
        
        ipconfig /all
🌞 Vérifier que les deux machines se joignent

        ping 10.10.10.225
        Envoi d’une requête 'Ping'  10.10.10.225 avec 32 octets de données :
        Réponse de 10.10.10.225 : octets=32 temps=1 ms TTL=128
        Réponse de 10.10.10.225 : octets=32 temps=1 ms TTL=128
        Réponse de 10.10.10.225 : octets=32 temps=3 ms TTL=128
        Réponse de 10.10.10.225 : octets=32 temps=4 ms TTL=128

🌞 Déterminer l'adresse MAC de votre correspondant

        arp -a
        Interface : 10.10.10.213 --- 0xa
        Adresse Internet      Adresse physique      Type
        10.10.10.225          08-8f-c3-36-61-47     dynamique
MAC : 08-8f-c3-36-61-47



    

