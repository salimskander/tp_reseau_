I. ARP

1. Echange ARP
🌞Générer des requêtes ARP

        PING 10.3.1.12 (10.3.1.12) 56(84) bytes of data.
        64 bytes from 10.3.1.12: icmp_seq=1 ttl=64 time=0.441 ms
        64 bytes from 10.3.1.12: icmp_seq=2 ttl=64 time=0.521 ms
        64 bytes from 10.3.1.12: icmp_seq=3 ttl=64 time=0.646 ms


table arp de marcel12

    ip n s
    10.3.1.11 dev enp0s8 lladdr 08:00:27:e0:68:b9 STALE

table arp de john11

    ip n s
    10.3.1.12 dev enp0s8 lladdr 08:00:27:72:0e:3d STALE


john11 :
    
    ip a
    enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:e0:68:b9

marcel12 :

    ip a
    enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:72:0e:3d


1. Analyse de trames
🌞Analyse de trames

utilisez la commande tcpdump pour réaliser une capture de trame
videz vos tables ARP, sur les deux machines, puis effectuez un ping


🦈 Capture réseau tp3_arp.pcapng qui contient un ARP request et un ARP reply

Si vous ne savez pas comment récupérer votre fichier .pcapng sur votre hôte afin de l'ouvrir dans Wireshark, et me le livrer en rendu, demandez-moi.


II. Routage
Vous aurez besoin de 3 VMs pour cette partie. Réutilisez les deux VMs précédentes.



Machine
10.3.1.0/24
10.3.2.0/24




router
10.3.1.254
10.3.2.254


john
10.3.1.11
no


marcel
no
10.3.2.12




Je les appelés marcel et john PASKON EN A MAR des noms nuls en réseau 🌻


   john                router              marcel
  ┌─────┐             ┌─────┐             ┌─────┐
  │     │    ┌───┐    │     │    ┌───┐    │     │
  │     ├────┤ho1├────┤     ├────┤ho2├────┤     │
  └─────┘    └───┘    └─────┘    └───┘    └─────┘



1. Mise en place du routage
🌞Activer le routage sur le noeud router

Cette étape est nécessaire car Rocky Linux c'est pas un OS dédié au routage par défaut. Ce n'est bien évidemment une opération qui n'est pas nécessaire sur un équipement routeur dédié comme du matériel Cisco.

🌞Ajouter les routes statiques nécessaires pour que john et marcel puissent se ping

il faut taper une commande ip route add pour cela, voir mémo
il faut ajouter une seule route des deux côtés
une fois les routes en place, vérifiez avec un ping que les deux machines peuvent se joindre



2. Analyse de trames
🌞Analyse des échanges ARP

videz les tables ARP des trois noeuds
effectuez un ping de john vers marcel

regardez les tables ARP des trois noeuds
essayez de déduire un peu les échanges ARP qui ont eu lieu
répétez l'opération précédente (vider les tables, puis ping), en lançant tcpdump sur marcel


écrivez, dans l'ordre, les échanges ARP qui ont eu lieu, puis le ping et le pong, je veux TOUTES les trames utiles pour l'échange

Par exemple (copiez-collez ce tableau ce sera le plus simple) :



ordre
type trame
IP source
MAC source
IP destination
MAC destination




1
Requête ARP
x

marcel AA:BB:CC:DD:EE

x
Broadcast FF:FF:FF:FF:FF



2
Réponse ARP
x
?
x

marcel AA:BB:CC:DD:EE



...
...
...
...




?
Ping
?
?
?
?


?
Pong
?
?
?
?




Vous pourriez, par curiosité, lancer la capture sur john aussi, pour voir l'échange qu'il a effectué de son côté.

🦈 Capture réseau tp3_routage_marcel.pcapng

3. Accès internet
    