# Sécurité
## Afficher les caractères invisibles
```Bash
cat -A script.sh
```

### Exemple sur un document à la ligne 197
```Bash
cat -A script.sh | sed -n 197p 
```

## Vérification de la signature d'un fichier
```Bash
sha1_sig='9ab59cddf97e52762039431d60567d6d69979f5f' ; check_sha1="$(sha1sum debian.iso | awk '{print $1}')" ; [[ $check_sha1 == $sha1_sig ]] && echo 'Signature valide' || echo 'Erreur de signature'
```

## Récupération des alertes 
```Bash
sudo dmesg | grep -Ei 'warning|critical|error|alert|invalid|failure|fatal'
sudo dmesg | grep -Ei 'warning|critical|error|alert|invalid|failure|fatal'
sudo grep -Ei 'warning|critical|error|alert|invalid|failure|fatal' /var/log/syslog 
```

## Indications pour sécuriser la configuration du service SSH
* Changer le port par défaut.
* Mise en place de Fail2Ban. 

### Modifier la configuration
```Bash
PermitRootLogin no
AllowUsers <nom_utilisateur>
PermitEmptyPasswords no
MaxAuthTries 3
Protocol 2
X11Forwarding no
AllowTcpForwarding no
```

## Mot de passe sur un fichier
### Chiffrer un fichier
```Bash
gpg -c fichier
```

### Pour déchiffrer
```Bash
gpg fichier.gpg
```

### Utiliser un autre algorithme
```Bash
gpg -c --cipher-algo nom_algorithme fichier
```

### Utilisation de la commande zip avec un mot de passe
```Bash
zip --password mot_de_passe fichier.zip fichier1 fichier2
```

### Extraction
```Bash
unzip archive_file.zip
```

### Utilisation de mcrypt

#### Lister les algorithmes
```Bash
mcrypt --list
```

#### Chiffrement du fichier
```Bash
mcrypt -a nom_algorithme fichier
```

#### Ouverture du fichier 
```Bash
mcrypt -d fichier.nc
```

### Pour les dossier 
```Bash
VeraCrypt
```

## Générer un hash MD5 d'une chaîne de caractères
```Bash
md5sum <<<"test"
```

## Scanner un réseau privé sans NMAP

Installation de arp-scan
```Bash
apt-get install arp-scan
```

Lancement du scan sur le réseau local
```Bash
arp-scan --interface=<interface réseau> --localnet
```

## Capturer des événement sur le réseau avec TCPdump
TCPdump est un outil d’écoute du réseau, il permet de capturer les événement sur le réseau, on peut
aussi enregistrer la capture dans un fichier compatible avec Wireshark.

Si on tape la commande tcpdump sans aucune option, cela va écouter sur l’ensemble du réseau tout
le trafic.

Exemple d’une ligne de résultat:
```Bash
14:19:02.843122 IP L875-10E.36343 > dns.google.domain: 25395+ PTR? 22.1.168.192.in-addr.arpa. (43)
```
* Chaque ligne représente la transmission d’un paquet.

||Description|
|---|---|
|14:19:02.843122|Heure d'arrivée du paquet sur l'interface réseau|
|IP|Type de protocole TCP, UDP, IGMP, ARP, ...|
|L875-10E|adresse source.|
|.36343|port source.|
|dns.google.domain|adresse de destination.|
|25395+|Port de déstination.|
|PTR?|Type de requête, ping,DNS,HTTP,FTP?PTR...|

Écouter le réseau avec TCPdump
```Bash
tcpdump -i enp1s0 -t icmp
```

## Chiffrer et déchiffrer des fichiers
```Bash
apt-get install ccrypt
```

Chiffrer le fichier file.txt
```Bash
ccrypt file.txt
```

Déchiffrer le fichier file.txt
```Bash
ccrypt -d file.txt
```

## Attaque DDOS sur un serveur Asterisk
```Bash
inviteflood enp1s0 6001 192.168.1.21 192.168.1.39 100
```

||Description|
|---|---|
|enp1s0|Interface réseau|
|6001|Utilisateur SIP cible|
|192.168.1.21|Serveur PBX|
|192.168.1.39|Adresse IP du téléphone cible|
|100|Envoi de 100 paquets|

Comportement observé:
1. Coupure de ligne.
2. Déconnexions d’utilisateurs.

## Mot de passe root perdu
* Récupérer une machine bloquée car on a perdu tous les identifiants.
1. Au moment de l'affichage du menu grub
appuyer sur "e"
2. chercher la ligne qui commence par "linux", et à la fin de cette ligne taper "init=/bin/bash" remplacer aussi "ro" par "rw" si besoin.
3. Taper F10 pour valider.
* On se retrouve dans une console en tant que root, on peut faire `passwd root` pour changer le mot de passe root.

## Quelques commandes NMAP
* Scan des 1000 ports les plus utilisés.
* L’option "-Pn" est conseillé car elle désactive la découverte des hôtes et oblige Nmap à
scanner chaque système comme s’il était actif.
```Bash
nmap -sC -sV -Pn 192.168.1.35
```

Pour effectuer un scan sur l’intégralité des ports il faut ajouter l’option “-p-”
```Bash
nmap -sC -sV -Pn -p- 192.168.1.35
```

Afficher les machines connectées sur le réseau
```Bash
nmap -sn 192.168.1.0/24
```

## Détecter et déchiffrer des connexions SIP avec SIPDUMP
Capture d’un login de session.
```Bash
sipdump -i enp1s0 logins.dump
```
* -i <interface> Permet d’indiquer une interface d’écoute.

* SIPCRACK permet de trouver les identifiants de connexion SIP.

Sur Kali Linux nous avons une liste de mots déjà prête du nom de "rockyou".
```Bash
cd /usr/share/wordlists/
```

On décompresse la liste de mots.
```Bash
gunzip rockyou.txt.gz
```

On lance le crack avec la liste de mots puis on indique quel est le login que nous voulons cracker puis
on valide:
```Bash
sipcrack -w /usr/share/wordlists/rockyou.txt logins.dump
```
* -w <liste de mots> liste de potentiels mot de passes, tous les mots de passes a essayer.

## Identification du serveur VoIP et des softphones ou téléphones Ip dans le réseau

```Bash
svmap 192.168.1.0/24
```

## Identification des numéros de ligne,(extension) sur le téléphone IP
```Bash
svwar -e1000-9000 192.168.1.39 -m INVITE --force
```

|Type d’authentification|Description|
|---|---|
|noauth|Extension qui ne requiert pas d’authentification.|
|reqauth|Extension qui requiert une authentification.|
|weird|L’extension existe probablement mais la réponse est
inattendue.|

Tentative pour déchiffrer le mot de passe.
```Bash
svcrack -u 6001 -d /usr/share/wordlists/rockyou.txt 192.168.1.39
```

Identification du serveur VoIP et des softphones ou téléphones IP dans le réseau.
Enregistrement d’un scan "SCAN2", (un scan du réseau à la recherche d'appareil SIP).
```Bash
svmap -s SCAN2 --randomize 192.168.1.0/24
```

Affichage de la liste des scans puis exportation d’un scan en document CSV.
```Bash
svreport list
```

```Bash
svreport export -f csv -o scan2.csv -s SAN2
```

Pour supprimer un scan.
```Bash
svreport delete -s scan1
```

## Visualiser et effacer les métadonnées d'un fichier
* Les fichiers que vous avez créé contiennent des données cachées.
* Mat (Metadata Anonymisation Toolkit) va nous permettre de visualiser les différentes traces
laissées sur nos fichiers.
* Il permet aussi d’effacer ces données pour rendre nos fichiers un peu
plus anonymes.

1. Installation de MAT.
```Bash
sudo apt-get install mat
```
|Commande|Description|
|---|---|
|`mat2 --list`|Liste les différents formats de fichiers supportés.|
|`mat2 --verbose`|Affiche des informations plus détaillées.|
|`mat2 --show`|Affiche les métadonnées d'un fichier.|
|`mat2 --help`|Afficher l’aide.|
|`mat2 --version`|Affiche la version du programme.|
|`mat2 --inplace`|Supprime les métadonnées.|

2. Afficher les métadonnées sans les supprimer.
```Bash
mat2 --show IMG_20200305_175904.jpg
```
3. Anonymisation des données.
* Supprime les métadonnées associées au fichier.
```Bash
mat2 --verbose --inplace IMG_20200305_175904.jpg
```

## Chiffrement d'une partition
 Création de la partition de travail chiffrée et définition du mot de passe.
```Bash
cryptsetup luksFormat /dev/sdX1
```
 
 Déverrouillage de la partition avec le mot de passe créé précédemment.
```Bash
cryptsetup open /dev/sdX3 cryptroot
```

## Rendre nos commandes invisibles à l'historique des commandes
```Bash
nano ~/.bashrc
```

```Bash
export HISTCONTROL=ignorespace
```

Puis écrire les commandes toujours avec un espace devant.
```Bash
   apt update
```

## On peut faire des scan sur cette adresse web légalement
```Bash
http://scanme.nmap.org/
```

## Changer son adresse MAC
```Bash
ip link show
```

```Bash
sudo ip link set dev enp1s0 down
```
 
```Bash
sudo ip link set dev enp1s0 address **:**:**:**:**:**
```

```Bash
sudo ip link set dev enp1s0 up
```

## Supprimer proprement un fichier avec la commande shred dans le but d'éviter une éventuelle récupération 
```bash
shred -fuvz -n 5 "fichier"
```
|Option|Description|
|---|---|
|-f|Change les permissions en écriture si nécessaire.|
|-u|Supprime le fichier après écrasement.|
|-v|Affiche la progression.|
|-z|Ajout de zéros après la suppression du fichier.|
|-n|Nombres de phases d'écrasement du fichier.|
 
## Créer un fichier chiffré
1. `vim -x fichier.txt`
2. Indiquer une clé de chiffrement puis valider.

 ## Afficher les identifiants wifi
 ```bash
 nmcli device wifi list
 
 nmcli device wifi show-password
 ```
