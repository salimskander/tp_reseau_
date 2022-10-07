Setup IP
----------

🌞 Mettez en place une configuration réseau fonctionnelle entre les deux machines

    netsh interface ipv4 set address name="Ethernet" static 10.10.10.213 255.255.252.0
mon ip : 10.10.10.213/22

son ip : 10.10.10.225/22

adresse de réseau : 10.10.8.0 (first adress)

adresse de broadcast : 10.10.11.255 (last adress)


🌞 Prouvez que la connexion est fonctionnelle entre les deux machines

    PS C:\WINDOWS\system32> ping 10.10.10.225

    Envoi d’une requête 'Ping'  10.10.10.225 avec 32 octets de données :
    Réponse de 10.10.10.225 : octets=32 temps=2 ms TTL=128
    Réponse de 10.10.10.225 : octets=32 temps=1 ms TTL=128
    Réponse de 10.10.10.225 : octets=32 temps=1 ms TTL=128
    Réponse de 10.10.10.225 : octets=32 temps=1 ms TTL=128

🌞 Wireshark it


ping = type 8 (demande d'echo)
pong = type 0 (reponse d'echo au type 8)

[ma capture de ping](./pingpong.pcapng)

🌞 Check the ARP table

    arp -a
                |
                |
                V

    Interface : 10.10.10.213 --- 0xa
    Adresse Internet      Adresse physique      Type
    10.10.10.225          08-8f-c3-36-61-47     dynamique

celle de votre réseau physique, WiFi, genre YNOV, car il n'y en a pas dans votre ptit LAN
c'est juste pour vous faire manipuler un peu encore :)


Il peut être utile de ré-effectuer des ping avant d'afficher la table ARP. En effet : les infos stockées dans la table ARP ne sont stockées que temporairement. Ce laps de temps est de l'ordre de ~60 secondes sur la plupart de nos machines.

🌞 Manipuler la table ARP

utilisez une commande pour vider votre table ARP
prouvez que ça fonctionne en l'affichant et en constatant les changements
ré-effectuez des pings, et constatez la ré-apparition des données dans la table ARP


Les échanges ARP sont effectuées automatiquement par votre machine lorsqu'elle essaie de joindre une machine sur le même LAN qu'elle. Si la MAC du destinataire n'est pas déjà dans la table ARP, alors un échange ARP sera déclenché.

    