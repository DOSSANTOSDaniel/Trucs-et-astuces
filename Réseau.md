# Réseau
## Diagnostique réseau
### La commande mtr permet d'afficher en continu les résultats d'envois de paquets avec des statistiques détaillées
```bash
mtr 8.8.8.8
```

## Affiche les adresses IP des différentes interfaces
```bash
hostname -I
```

## Test d'ouverture d'un port
### Avec la commande telnet
```bash
telnet localhost 22
```

### Avec la commande netcat
```bash
nc -zv localhost 22
```

### Autre méthode
```bash
(echo > /dev/tcp/127.0.0.1/22) 2> /dev/null && echo "Port en écoute" || echo "Port fermé"
```

## Afficher les serveurs samba dans un réseau 
```bash
findsmb
```

## Sauvegarde avec RSYNC sur le réseau
```Bash
rsync -avrz -e 'ssh -p 22' --progress --partial --stats file.txt root@192.168.1.112:/home/daniel/Images
```

## Scanner le réseau local et afficher le nom et l'adresse IP des machines connectées
```Bash
for i in 192.168.1.{1..254}; do ping -c 1 -W 1 $i > /dev/null && (echo "IP: $i" && echo "NAME : $(host $i | awk '{print $NF}')" && echo "-----------------------------------") ; done
```

### En utilisant la commande arp-scan
```Bash
for i in 192.168.1.{1..80}; do ping -c 1 -W 1 $i > /dev/null && (echo "IP: $i" && echo "NAME : $(host $i | awk '{print $NF}')" && echo "OTHER : $(sudo arp-scan $i | sed -n '3p')" && echo "-----------------------------------") ; done
```

## Afficher les interfaces réseau et leur adresse MAC
```Bash
┌──[daniel👾archos]-(~)
│
└─$ ip -br -c link show

lo               UNKNOWN        00:00:00:00:00:00 <LOOPBACK,UP,LOWER_UP> 
enp8s0           UP             xx:xx:xx:xx:xx:xx <BROADCAST,MULTICAST,UP,LOWER_UP> 
virbr0           DOWN           52:54:00:70:a6:b5 <NO-CARRIER,BROADCAST,MULTICAST,UP>
virbr0-nic       DOWN           52:54:00:70:a6:b5 <BROADCAST,MULTICAST> 
```

## L'application warp
* Permet des transfères de fichiers chiffrés entre machines en LAN ou WAN.
- https://flathub.org/apps/details/app.drey.Warp
- https://gitlab.gnome.org/World/warp

## Afficher l’IP publique en ligne de commande
### Avec wget
```Bash
wget -qO- http://ipv4.icanhazip.com/
```

```Bash
wget -qO- icanhazip.com
```

### Avec curl
```Bash
curl ifconfig.co
```

```Bash
curl ifconfig.me
```

```Bash
curl icanhazip.com
```

```Bash
curl ipinfo.io
```

## Calcul de sous réseaux en ligne de commande
### Installation de ipcalc
```Bash
$ sudo apt install ipcalc -y
```

### Informations à propos du réseau (ex: 192.168.0.0/24)
```Bash
$ ipcalc 192.168.20.0/24

Address:   192.168.20.0         11000000.10101000.00010100. 00000000
Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
=>
Network:   192.168.20.0/24      11000000.10101000.00010100. 00000000
HostMin:   192.168.20.1         11000000.10101000.00010100. 00000001
HostMax:   192.168.20.254       11000000.10101000.00010100. 11111110
Broadcast: 192.168.20.255       11000000.10101000.00010100. 11111111
Hosts/Net: 254
```

### Affichage sans les données en binaire
```Bash
$ ipcalc -b 192.168.20.100

Address:   192.168.20.100 
Netmask:   255.255.255.0 = 24   
Wildcard:  0.0.0.255            
=>
Network:   192.168.20.0/24      
HostMin:   192.168.20.1         
HostMax:   192.168.20.254       
Broadcast: 192.168.20.255       
Hosts/Net: 254                   Class C, Private Internet
```

### Afficher la classe (CIDR)
```Bash
$ ipcalc -c 192.25.154.38

24
```

### Afficher les informations au format HTML
```Bash
$ ipcalc -h 192.168.20.100 > tyty.html && firefox tyty.html
```

### Diviser une adresse réseau en plusieurs sous réseaux selon un nombre de postes donnés par réseau

#### Un exemple avec le réseau 192.168.1.0/24
- On veut 3 sous réseaux
   - Pour le premier sous-réseau on a besoin de 12 postes
   - Pour le deuxième sous-réseau on a besoin de 22 postes
   - Pour le troisième sous-réseau on a besoin de 32 postes

```Bash
$ ipcalc 192.168.1.0/255.255.255.0 -s 12 22 32 

Address:   192.168.1.0          11000000.10101000.00000001. 00000000
Netmask:   255.255.255.0 = 24   11111111.11111111.11111111. 00000000
Wildcard:  0.0.0.255            00000000.00000000.00000000. 11111111
=>
Network:   192.168.1.0/24       11000000.10101000.00000001. 00000000
HostMin:   192.168.1.1          11000000.10101000.00000001. 00000001
HostMax:   192.168.1.254        11000000.10101000.00000001. 11111110
Broadcast: 192.168.1.255        11000000.10101000.00000001. 11111111
Hosts/Net: 254                   Class C, Private Internet

1. Requested size: 12 hosts
Netmask:   255.255.255.248 = 29 11111111.11111111.11111111.11111 000
Network:   192.168.1.96/29      11000000.10101000.00000001.01100 000
HostMin:   192.168.1.97         11000000.10101000.00000001.01100 001
HostMax:   192.168.1.102        11000000.10101000.00000001.01100 110
Broadcast: 192.168.1.103        11000000.10101000.00000001.01100 111
Hosts/Net: 6                     Class C, Private Internet

2. Requested size: 22 hosts
Netmask:   255.255.255.224 = 27 11111111.11111111.11111111.111 00000
Network:   192.168.1.64/27      11000000.10101000.00000001.010 00000
HostMin:   192.168.1.65         11000000.10101000.00000001.010 00001
HostMax:   192.168.1.94         11000000.10101000.00000001.010 11110
Broadcast: 192.168.1.95         11000000.10101000.00000001.010 11111
Hosts/Net: 30                    Class C, Private Internet

3. Requested size: 32 hosts
Netmask:   255.255.255.224 = 27 11111111.11111111.11111111.111 00000
Network:   192.168.1.0/27       11000000.10101000.00000001.000 00000
HostMin:   192.168.1.1          11000000.10101000.00000001.000 00001
HostMax:   192.168.1.30         11000000.10101000.00000001.000 11110
Broadcast: 192.168.1.31         11000000.10101000.00000001.000 11111
Hosts/Net: 30                    Class C, Private Internet

Needed size:  112 addresses.
Used network: 192.168.1.0/24.999999999999999999999999999999999999999
Unused:
192.168.1.112/28
192.168.1.128/25
```

## Afficher le contenu du cache DNS sur Ubuntu
- C'est l'alternative à la commande 'ipconfig /displaydns' sur Windows.
### Libérer les informations du cache DNS
- Quand systemd-resolved reçoit le signal USR1, il va rendre accessible les informations des enregistrements du cache DNS dans les journaux du système.
```Bash
sudo systemctl kill --signal='USR1' systemd-resolved
```

### Affichage du contenu du cache DNS
```Bash
sudo journalctl -b -u systemd-resolved --grep=" IN " --no-pager --no-hostname
```

|Option|Description|
|---|---|
|-b|Les journaux depuis le démarrage courant seront affichés, pour tous les démarrages '-b all'.|
|--grep=" IN "|Recherche l'occurrence  " IN ".|
|--no-pager|Mode non interactif, par défaut cela utilise less pour afficher les données.|
|--no-hostname|N'affiche pas le hostname de la machine.|

### Effacer le contenu du cache DNS
- C'est l'alternative à la commande 'ipconfig /flushdns' sur Windows
```Bash
sudo systemd-resolve --flush-caches
```

## Éditer un fichier au travers d'internet
###  Avec la commande ssh
```Bash
ssh -p 2244 daniel@192.168.1.85 -t sed -i 's/Bonjour/Bonsoir/g' /home/daniel/exemple.txt
```
 
### Exemple avec la commande scp
```Bash
$ vim scp://daniel@192.168.1.85:2244//home/daniel/exemple.txt
#           ^             ^                   ^                ^                              ^
#            |              |                    |                 |                               |
#           v              |                    |                 v                              |
# Protocole      v                    |              Port                          v
#                Hostname            v                                  Chemin du fichier
#                                IP de la machine                                          
```

### Exemple avec la commande sftp
```Bash
vim sftp://daniel@192.168.1.85//home/daniel/toto/exemple.txt
```

### Exemple avec la commande rcp
```Bash
vim rcp://daniel@192.168.1.85//home/daniel/toto/exemple.txt
```

### Exemple en HTTPS 
```Bash
vim https://daniel@192.168.1.85//home/daniel/toto/exemple.txt
```

## Accéder aux fichiers d'une machine distante en utilisant le gestionnaire de fichiers Gnome
1. Ouvrir le gestionnaire de fichiers.
2. Cliquer sur +Autres emplacements.
3. Insérer la ligne suivante `ssh://192.168.1.85:2244/home/daniel` en bas.
4. Cliquer sur entrée 

## Lancer une application graphique d'une machine distante sur une machine locale
```Bash
ssh -X -t daniel@192.168.1.40 'firefox'
```

## Utilisation de wget
### Pour spécifier le dossier de téléchargement.
```Bash
wget -P dossier_de_téléchargement http://ftp.crifo.org/debian-cd/current/amd64/iso-cd/debian-11.5.0-amd64-netinst.iso
```

### Pour spécifier le nom du fichier de sortie
```Bash
wget -O debian.iso http://ftp.crifo.org/debian-cd/current/amd64/iso-cd/debian-11.5.0-amd64-netinst.iso
```

### Reprendre un téléchargement interrompu
```Bash
wget -c http://ftp.crifo.org/debian-cd/current/amd64/iso-cd/debian-11.5.0-amd64-netinst.iso
```

### Limiter le débit
```Bash
wget --limit-rate 300K http://ftp.crifo.org/debian-cd/current/amd64/iso-cd/debian-11.5.0-amd64-netinst.iso
```
* Limitation de 300KB par secondes.

### Téléchargement en tache de fond
```Bash
wget -b http://ftp.crifo.org/debian-cd/current/amd64/iso-cd/debian-11.5.0-amd64-netinst.iso
```
* Pour voire l'avancement consulter wget-log.

## Afficher l'activité des services réseau en temps réel
```Bash
lsof -i
```

## Afficher les applications qui utilisent internet
```Bash
lsof -P -i -n
ss -p
```

## Partager un fichier au travers d'internet sur le port 8000
* Sur la machine A.
```Bash
nc -v -l 8000 < fichier.txt
```

* Sur la machine B
```Bash
nc 192.168.1.42 8000
```

## Alternative à SSH
```bash
mosh
```

## Maintenir la connexion SSH
```bash
ssh -o "ServerAliveInterval 60" user@domain
```

## Exécuter plusieurs commandes sur plusieurs machines en même temps
### Installation de l'application clusterssh
```Bash
sudo apt-get install clusterssh
```

### Configuration 
```Bash
vim /etc/clusters

groupe_1 = machine1 machine2
groupe_2 = machine3 machine4

all = groupe_1 groupe_2
```

### Connexion aux machines
```Bash
cssh -l <username> <clustername>
```

## interface web terminal ssh(Shell In A Box)
### installation
```Bash
sudo apt install openssl shellinabox
```
* Shellinaboxd écoute sur le port 4200 en TCP : https://votre IP:4200

## Effacer un host ssh
```Bash
ssh-keygen -R 192.168.1.22
```

## Afficher la météo sur le terminal
```bash
curl wttr.in
```

### Affichage de l'aide
```bash
curl wttr.in/:help
```

### Météo en français dans la ville de Massy
```bash
curl fr.wttr.in/Massy
```

### Les phases de la lune actuellement en France
```bash
curl fr.wttr.in/moon@+France
```

### Avec options
```bash
curl fr.wttr.in/Massy?0QF
```
|Option|Description|
|---|---|
|0|Affiche seulement la météo de la journée en cours.|
|Q|Affichage minimal.|
|F|Ne pas afficher la phrase de publicité|

### Création d'une image au format png, cette image est le résultat de la météo à Massy
```bash
curl fr.wttr.in/Massy.png --output Massy.png
```

```bash
curl -O fr.wttr.in/Massy.png
```

### Image avec transparence
```bash
curl fr.wttr.in/Massy_transparency=50.png --output Massy.png
```
- Transparence de 0 à 255.

## Télécharger des objets comme avec wget
```bash
curl -L -O url
```
|Option|Description|
|---|---|
|L|Suivre une redirection.|
|O|Sauvegarde le fichier avec le nom source.|

## Carte du monde en ligne de commande
```Bash
telnet mapscii.me
```

## Partager le terminal via LAN ou WAN
### En local avec les commandes
- screen
- tmux

### Par internet 
* [tmate](https://tmate.io/)
* [teleconsole](https://www.teleconsole.com/)
* [tsl0922](https://tsl0922.github.io/ttyd/)

## Tester le fichier de configuration de HAproxy avant de démarrer le serveur
```Bash
haproxy -f /path/to/haproxy.cfg -c
```

## Afficher les ordinateurs possédant un nom netbios sur un réseau
```bash
sudo nbtscan -r 192.168.0.0/24

sudo nbtscan -rv 192.168.0.0/24

sudo nbtscan 192.168.0.1-254

sudo nbtscan -v -s : 192.168.1.0/24
```

## Afficher le nom netbios et le rôle d'un ordinateur
```Bash
nmblookup -A 192.168.0.17

Looking up status of 192.168.0.17
    DANIEL-PC   	<00> -     	B <ACTIVE>
    WORKGROUP   	<00> - <GROUP> B <ACTIVE>
    DANIEL-PC   	<20> -     	B <ACTIVE>
    WORKGROUP   	<1e> - <GROUP> B <ACTIVE>
```
|Rôle|Description|
|---|---|
|<00>|Station de travail.|
|<03>|Service de messagerie.|
|<20>|Serveur.|
|<1B>|Contrôleur de domaine.|
|<1C>, <1D>|Explorateur de domaine.|

Autres commandes pour scanner le réseau.
```Bash
nmap --script smb-os-discovery.nse -p445 192.168.1.0/24
```

## Ajouter une bannière à la connexion SSH
Création et édition du fichier sshd-banner.
```Bash
sudo vim /etc/ssh/sshd-banner
```

Configuration de la bannière dans le fichier de configuration de SSH, placer cette ligne dedans
"Banner /etc/ssh/sshd-banner".
```Bash
sudo vim /etc/sshd/sshd_config
```

```Bash
sudo systemctl restart sshd
```

## Afficher la table ARP avec cat
```Bash
cat /proc/net/arp
```

## Afficher les connexions actives sur un poste
Trouver les processus spécifiques en écoute sur un port.
```Bash
lsof -i TCP:80
```

Pour le trafic en TCP
```Bash
lsof -i TCP
```

Pour le trafic en UDP
```Bash
lsof -i UDP
```

Les connections établies sur un poste distant.
```Bash
lsof -i @192.168.1.22
```

## Lancer des applications distantes,(graphiquement) via SSH

* X Forwarding

1. Modifier le fichier /etc/ssh/sshd_config.
```Bash
X11Forwarding yes
```

2. Lancer une application qui est installée sur la machine distante et avoir l'affichage sur la machine locale.
```Bash
ssh -XC daniel@192.168.1.22 gedit
```

Pour lancer une application installée sur la machine distante avec un affichage sur la machine distante.
```Bash
ssh -XC daniel@192.168.1.22 export DISPLAY=:0.0 gedit
```

La variable DISPLAY permet de d'afficher le numéro d'affichage de l'écran principal.
```Bash
echo $DISPLAY
```

## Déconnecter une session SSH
```Bash
who
```

```Bash
w
```

```Bash
ps ax | grep pts/0
```

```Bash
kill -9 3227
```
* Chaque utilisateur a un pts/x

## Activer le mode promiscuité
Afficher les interfaces.
```Bash
ip l
```

```Bash
ip link set enp1s0 promisc on
```

|Drapeaux|Description|
|---|---|
|BROADCAST:|Broadcast.|
|MULTICAST:|Multicast.|
|PROMISC:|Mode promiscuité.|
|R:|Indique que l’interface est démarré.|
|UP:|Indique que l’interface est initialisé.|

## Détection d’un conflit d’adresses IP dans un réseau
### Alimenter la table ARP
Normalement il faudrait faire un ping de diffusion broadcast pour alimenter la table ARP mais la plupart des machines sous Linux ignorent le ping broadcast(cat /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts).

On va donc faire un ping manuel sur chaque machine du réseau avec deux méthodes.
1. Utiliser la commande ping (~5 minutes). 
```Bash
for i in 192.168.1.{1..254}; do ping -n -c 1 -W 1 $i; done
```
   * La commande tcpdump pour visualiser l'envoi de paquets icmp sur le réseau.
```Bash
sudo tcpdump -i enp1s0 icmp
```
2. Utiliser la commande arp-scan (~3 secondes).
```Bash
sudo arp-scan -I enp1s0 -l
```

### Afficher la table ARP
```Bash
ip neighbour
```

## Afficher la liste des port disponibles sous Linux
```Bash
nano /etc/services
```

## Télécharger un dépôt dans un dossier en particulier(/var/www/html/)
```Bash
git -C /var/www/html/ clone https://github.com/ethicalhack3r/DVWA.git
```

## Afficher le résultat d'une commande sur un navigateur web ou terminal
```Bash
while true; do timeout 1 nc -w 1 -l 8844 <<< $(hostnamectl); done
```

```Bash
nc 127.0.0.1 8844
```
* Consulter aussi http://127.0.0.1:8844/

## La commande ping
Indiquer le nombre de paquets ICMP à envoyer, exemple avec 5 paquets.
```Bash
ping -c 5 192.168.1.48
```

Indiquer une limite de temps pour l'envoi de l'ensemble de tous les paquets ICMP, exemple avec 3 secondes.
```Bash
ping -w 3 192.168.1.48
```
*Envoi tous les paquets ICMP possibles durant 3 secondes.

Changer le temps d’intervalle entre chaque paquet ICMP envoyé, exemple avec 0.1 secondes,(0.1 secondes = 100 ms)
```Bash
ping -i 0.1 192.168.1.48
```

Changer la taille du paquet ICMP, la taille par défaut est de 64 bytes = 64 octets sur Linux et 32 octets sur Windows.
```Bash
ping -s 80 192.168.1.48
```

## Connexion SSH en mode non interactif avec mot de passe
```Bash
apt install sshpass

sshpass -P <mot de passe> ssh daniel@192.168.1.22 "ls -l"
```

## Récupération de certaines informations du système avec `cat`
### Connections TCP
```bash
cat  /proc/net/tcp
```

### Connections UDP
```bash
cat  /proc/net/udp
```

### Affiche l'adresse MAC
* Interface internet a adapter(eth0)
```bash
cat /sys/class/net/eth0/address
```

### Affiche la table ARP
```bash
cat  /proc/net/arp
```

### Informations sur les interfaces réseau
```bash
cat  /proc/net/route
```

### Informations sur l'interface wifi
```bash
cat  /proc/net/wireless
```

## Montage d'un dossier ou d'un système de fichiers au travers de SSH

* Sur la Machine locale :
```Bash
mkdir /home/daniel/remote_host_ananas
sshfs ananas@192.168.1.42:/home/ananas /home/daniel/remote_host_ananas -o idmap=user -o reconnect -o sshfs_sync
```

### Vérification du montage
```Bash
ps -aux | grep sshfs
findmnt | grep remote_host_ananas
mount | grep remote_host_ananas
```

### Pour démonter le montage
```Bash
fusermount -u /home/daniel/remote_host_ananas
```

## Sauvegarde d'un périphérique dans un fichier sur un poste distant          via SSH
```Bash
dd if=/dev/sda | ssh daniel@192.168.1.22 'dd of=sda.img'
```