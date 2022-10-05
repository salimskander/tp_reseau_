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

Utilisation d'un des deux comme gateway
-------------

🌞Tester l'accès internet
        ping 1.1.1.1

        Envoi d’une requête 'Ping'  1.1.1.1 avec 32 octets de données :
        Réponse de 1.1.1.1 : octets=32 temps=24 ms TTL=55
        Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=55
        Réponse de 1.1.1.1 : octets=32 temps=24 ms TTL=55
        Réponse de 1.1.1.1 : octets=32 temps=21 ms TTL=55

🌞 Prouver que la connexion Internet passe bien par l'autre PC
        
                tracert 1.1.1.1

        Détermination de l’itinéraire vers 1.1.1.1 avec un maximum de 30 sauts.

        1    <1 ms    <1 ms    <1 ms  DESKTOP-K74UV6N [192.168.137.1]
        2     *        *        *     Délai d’attente de la demande dépassé.
        3     4 ms     5 ms     3 ms  10.33.19.254
        4     5 ms     3 ms     6 ms  77.196.149.137
        5     9 ms     9 ms    94 ms  212.30.97.108
        6    21 ms    22 ms    22 ms  77.136.172.222
        7    22 ms    20 ms    26 ms  77.136.172.221
        8    24 ms    27 ms    22 ms  77.136.10.221
        9    22 ms    22 ms    24 ms  77.136.10.221
        10    44 ms    22 ms    22 ms  141.101.67.254
        11    64 ms    25 ms    22 ms  172.71.132.2
        12    23 ms    26 ms    23 ms  1.1.1.1
Petit chat privé
-------------
🌞 sur le PC serveur avec par exemple l'IP 10.10.10.213

        PS C:\Users\pc\Downloads\netcat-1.11> ping 10.10.10.210

        Envoi d’une requête 'Ping'  10.10.10.210 avec 32 octets de données :
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.210 : octets=32 temps<1ms TTL=128

        Statistiques Ping pour 10.10.10.210:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
        Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
        PS C:\Users\pc\Downloads\netcat-1.11> .\nc.exe -l -p 8888
        mec
        ftg
        tg
        bouffon
        fdp
        chu mort ca marche
        c bi1





🌞 sur le PC client avec par exemple l'IP 192.168.1.2

        ping 10.10.10.213

        Envoi d’une requête 'Ping'  10.10.10.213 avec 32 octets de données :
        Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128
        Réponse de 10.10.10.213 : octets=32 temps<1ms TTL=128

        Statistiques Ping pour 10.10.10.213:
        Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
        Durée approximative des boucles en millisecondes :
        Minimum = 0ms, Maximum = 0ms, Moyenne = 0ms
        PS C:\Users\cedri\Desktop\netcat-1.11> ^C
        PS C:\Users\cedri\Desktop\netcat-1.11> .\nc.exe 10.10.10.213 8888
        ftg
        mec
        tg
        bouffon
        fdp
        chu mort ca marche
        c bi1

🌞 Visualiser la connexion en cours
        netstat -a -n -b
        TCP    10.10.10.225:8888      10.10.10.210:54361     ESTABLISHED
        [nc.exe]

🌞 Pour aller un peu plus loin

- si vous faites un netstat sur le serveur AVANT que le client netcat se connecte, vous devriez observer que votre serveur netcat écoute sur toutes vos interfaces
        .\nc.exe -l -p 8888

        netstat -a -n -b | select-string 8888

        TCP    0.0.0.0:8888           0.0.0.0:0              LISTENING

- il est possible d'indiquer à netcat une interface précise sur laquelle écouter
  - par exemple, on écoute sur l'interface Ethernet, mais pas sur la WiFI
        .\nc.exe -l -p 8888 -s 10.10.10.225

        netstat -a -n -b | select-string 8888

        TCP    10.10.10.225:8888      0.0.0.0:0              LISTENING





    

