# Linux_Network_Basics

Lab sur les bases du réseau réalisé sur une VM Linux Mint.

## Objectif

Comprendre comment afficher et analyser les interfaces réseau sur Linux.

## Environnement

- VM Linux Mint
- VirtualBox

## Commandes étudiées

### ip a 

```bash
ip a
```

Cette commande permet d'afficher les interfaces réseau de la machine ainsi que les adresses IP associées. 

Elle permet notamment d'observer : 

- les interfaces réseau (lo, enp0s3)
- l'adresse IP de la machine
- le masque de sous-réseau
- l'état de l'interface réseau

## Exemple de résultat 
[Résultat de la commande ip a](images/ip_a.png)

La commande affiche deux interfaces principales : 

**lo (loopback)**
Interface locale utilisée pour la communication interne de la machine. (127.0.0.1/8)

**enp0s3**
Interface réseau principale de la VM avec une adresse IP privée. (192.168.1.x)

## Conclusion

La commande `ip a` est essentielle pour analyser la configuration réseau d'une machine. 

### ip route

```bash
ip route
```

Cette commande permet d'afficher la table de routage de la machine.
Elle indique par où les paquets doivent passer pour atteindre le réseau.

### Exemple de résultat 
[Résultat de la commande ip route](images/ip_route.png)

default via 192.168.1.254 dev enp0s3
192.168.1.0/24 dev enp0s3 src 192.168.1.199

## Analyse

La machine appartient au réseau local :
192.168.1.0/24 

Son adresse IP est :
192.168.1.xxx

La passerelle utilisée pour accéder aux réseaux externes est : 
192.168.1.xxx

VM Linux
192.168.1.xxx
      |
      | réseau local
      |
Routeur
192.168.1.254
      |
      | Internet 
      |
Google 
8.8.8.8


## Test de connectivité

### Commande utilisée : 
[Résultat de la commande ping 8.8.8.8](iamges/test_ping)

```bash
ping 8.8.8.8
```

La machine peut atteindre l'adresse IP 8.8.8.8, ce qui confirme que la connectivité à internet fonctionne.
Le protocole utilisé est le protocole ICMP (Internet Control Message Protocol)


## Test de résolution DNS 

### Commande utilisée : 
[Résulat de la commande ping google.com](images/ping_google.com)

```bash
ping google.com
```
La commande retourne : Nom ou service inconnu.
Cela indique que la machine n'arrive pas à résoudre le nom de domaine google.com en adresse IP.

## vérification du serveur DNS 

### Commande utilisée : 

```bash
resolvectl status
```

Résultat observé : DNS Server : 192.168.1.x

## Solution 

Ajout d'un serveur DNS public.

### Commande utilisée : 

```bash
sudo resolvectl dns enp0s3 8.8.8.8
```


## Analyse des ports ouverts 

### Commande utilisée : 
[Résultat de la commande ss_tuln](images/ss_tiln.png)

```bash
ss -tuln
```

Résultat : 

Plusieurs ports sont ouverts sur la machine.

Port 53 : 
Service DNS local utilisé par systemd-resolved.

Port 68 : 
Client DHCP permettant d'obtenir automatiquement une adresse IP.

Port 5353 : 
Service mDNS utilisé pour la découverte réseau locale.

Port 631 : 
Service d'impression CUPS accessible uniquement en local.

## Conclusion

Ce lab permet de comprendre plusieurs concepts fondamentaux du réseau sous Linux :

- identification des interfaces réseau
- analyse de la table de routage
- test de connectivité avec ICMP
- diagnostic d'un problème DNS
- observation des services réseau actifs

Ces commandes sont essentielles pour le diagnostic réseau et constituent une base importante pour l'administration système et la cybersécurité.
