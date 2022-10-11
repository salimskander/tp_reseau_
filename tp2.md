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


```
arp -a
```
La gateway de notre réseau :
```
 Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
```




🌞 **Manipuler la table ARP**
```
arp -d
```
```
 arp -a

Interface : 10.33.19.192 --- 0xb
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  224.0.0.22            01-00-5e-00-00-16     statique

Interface : 10.10.10.225 --- 0xe
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.56.1 --- 0x12
  Adresse Internet      Adresse physique      Type
  224.0.0.22            01-00-5e-00-00-16     statique
  ```
```
ping 10.10.10.213

Envoi d’une requête 'Ping'  10.10.10.213 avec 32 octets de données :
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128
Réponse de 10.10.10.213 : octets=32 temps=1 ms TTL=128

Statistiques Ping pour 10.10.10.213:
    Paquets : envoyés = 4, reçus = 4, perdus = 0 (perte 0%),
Durée approximative des boucles en millisecondes :
    Minimum = 1ms, Maximum = 1ms, Moyenne = 1ms
PS C:\Windows\system32> arp -a

Interface : 10.33.19.192 --- 0xb
  Adresse Internet      Adresse physique      Type
  10.33.19.254          00-c0-e7-e0-04-4e     dynamique
  10.33.19.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.192.152.143       01-00-5e-40-98-8f     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 10.10.10.225 --- 0xe
  Adresse Internet      Adresse physique      Type
  10.10.10.213          40-b0-34-f0-e5-0e     dynamique
  10.10.11.255          ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.192.152.143       01-00-5e-40-98-8f     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique

Interface : 192.168.56.1 --- 0x12
  Adresse Internet      Adresse physique      Type
  192.168.56.255        ff-ff-ff-ff-ff-ff     statique
  224.0.0.22            01-00-5e-00-00-16     statique
  239.192.152.143       01-00-5e-40-98-8f     statique
  239.255.255.250       01-00-5e-7f-ff-fa     statique
  ```


🌞 **Wireshark it**

🦈 **PCAP qui contient les trames ARP**

[ma capture des trames arp](./trames_arp.pcapng)

# II.5 Interlude hackerzz

**Chose promise chose due, on va voir les bases de l'usurpation d'identité en réseau : on va parler d'*ARP poisoning*.**

> On peut aussi trouver *ARP cache poisoning* ou encore *ARP spoofing*, ça désigne la même chose.

Le principe est simple : on va "empoisonner" la table ARP de quelqu'un d'autre.  
Plus concrètement, on va essayer d'introduire des fausses informations dans la table ARP de quelqu'un d'autre.

Entre introduire des fausses infos et usurper l'identité de quelqu'un il n'y a qu'un pas hihi.

---

➜ **Le principe de l'attaque**

- on admet Alice, Bob et Eve, tous dans un LAN, chacun leur PC
- leur configuration IP est ok, tout va bien dans le meilleur des mondes
- **Eve 'lé pa jonti** *(ou juste un agent de la CIA)* : elle aimerait s'immiscer dans les conversations de Alice et Bob
  - pour ce faire, Eve va empoisonner la table ARP de Bob, pour se faire passer pour Alice
  - elle va aussi empoisonner la table ARP d'Alice, pour se faire passer pour Bob
  - ainsi, tous les messages que s'envoient Alice et Bob seront en réalité envoyés à Eve

➜ **La place de ARP dans tout ça**

- ARP est un principe de question -> réponse (broadcast -> *reply*)
- IL SE TROUVE qu'on peut envoyer des *reply* à quelqu'un qui n'a rien demandé :)
- il faut donc simplement envoyer :
  - une trame ARP reply à Alice qui dit "l'IP de Bob se trouve à la MAC de Eve" (IP B -> MAC E)
  - une trame ARP reply à Bob qui dit "l'IP de Alice se trouve à la MAC de Eve" (IP A -> MAC E)
- ha ouais, et pour être sûr que ça reste en place, il faut SPAM sa mum, genre 1 reply chacun toutes les secondes ou truc du genre
  - bah ui ! Sinon on risque que la table ARP d'Alice ou Bob se vide naturellement, et que l'échange ARP normal survienne
  - aussi, c'est un truc possible, mais pas normal dans cette utilisation là, donc des fois bon, ça chie, DONC ON SPAM

![Am I ?](./pics/arp_snif.jpg)

---

➜ J'peux vous aider à le mettre en place, mais **vous le faites uniquement dans un cadre privé, chez vous, ou avec des VMs**

➜ **Je vous conseille 3 machines Linux**, Alice Bob et Eve. La commande `[arping](https://sandilands.info/sgordon/arp-spoofing-on-wired-lan)` pourra vous carry : elle permet d'envoyer manuellement des trames ARP avec le contenu de votre choix.

GLHF.

# III. DHCP you too my brooo

🌞 **Wireshark it**

🦈 **PCAP qui contient l'échange DORA**
```
 netsh interface ipv4 set address name="Wi-Fi" static 81.10.10.225 255.255.0.0
 ```
```
 netsh interface ipv4 set address name="Wi-Fi" dhcp 
 ```
[ma capture de l'échange DORA](./echange_dora.pcapng)

    