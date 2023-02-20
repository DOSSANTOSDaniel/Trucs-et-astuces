# Système et autre
## Récupération de certaines informations du système avec `cat`
### Afficher l'UID de l'utilisateur courant
```bash
cat /proc/self/loginuid
```

### Afficher le type de l'OS
```bash
cat /proc/sys/kernel/ostype
```

### Afficher la version du noyau Linux
```bash
cat /proc/sys/kernel/osrelease
```

### Version de la distribution
```bash
cat /proc/sys/kernel/version
```

## Afficher des information sur un utilisateur
### Avec la commande `pinky`
```bash
┌──[daniel🐧iS3810]-(~)
│
└─$ pinky -l daniel

Identifiant : daniel                      Nom réel :  daniel
Répertoire : /home/daniel                 Interpréteur :  /bin/bash
```

## Afficher le nombre maximal et le minimal de comptes utilisateurs possibles 
```Bash
$ grep -E '^UID_MIN|^UID_MAX' /etc/login.defs

UID_MIN			 1000
UID_MAX			60000
``` 

## Afficher les comptes utilisateurs
```Bash
getent passwd {1000..60000}
``` 

## Afficher le UID,GUID et autres groupes associés à l'utilisateur courant
```bash
┌──[daniel🐧iS3810]-(~)
│
└─$ id daniel

uid=1000(daniel) gid=1000(daniel) groupes=1000(daniel),4(adm),20(dialout),24(cdrom),27(sudo),30(dip),46(plugdev),108(kvm),121(lpadmin),131(lxd),132(sambashare),134(libvirt),136(docker)
```

## Affiche le nom de l'utilisateur courant
```Bash
id -un
```

## Affiche des informations sur le matériel
```bash
inxi -Fx
```

## Informations sur le matériel
### Utilisation de la commande `sysctl`
```Bash
sudo sysctl -a hw.model
sudo sysctl -a | grep -i hw.*cpu
sudo sysctl -n net.wlan.devices   # Information sur l'interface Wifi
```

### Utilisation de la commande `dmesg`
```Bash
sudo dmesg | grep CPU
sudo dmesg | grep memory
```

### Savoir quand un système a été redémarré ou arrêté pour la dernière fois
```Bash
sudo last -1 reboot
sudo last -1 shutdown
```

### Savoir qui est connecté au système
```Bash
w
who
users
```

### Utilisation de la commande `dmidecode`
```Bash
sudo dmidecode
sudo dmidecode -t processor
sudo dmidecode -t memory
sudo dmidecode -t bios
sudo dmidecode -t 0
```

#### Tableau des codes a utiliser avec la commande `dmidecode`
```Bash
Code 	Description
0 	BIOS
1 	System
2 	Baseboard
3 	Chassis
4 	Processor
5 	Memory Controller
6 	Memory Module
7 	Cache
8 	Port Connector
9 	System Slots
10 	On Board Devices
11 	OEM Strings
12 	System Configuration Options
13 	BIOS Language
14 	Group Associations
15 	System Event Log
16 	Physical Memory Array
17 	Memory Device
18 	32-bit Memory Error
19 	Memory Array Mapped Address
20 	Memory Device Mapped Address
21 	Built-in Pointing Device
22 	Portable Battery
23 	System Reset
24 	Hardware Security
25 	System Power Controls
26 	Voltage Probe
27 	Cooling Device
28 	Temperature Probe
29 	Electrical Current Probe
30 	Out-of-band Remote Access
31 	Boot Integrity Services
32 	System Boot
33 	64-bit Memory Error
34 	Management Device
35 	Management Device Component
36 	Management Device Threshold Data
37 	Memory Channel
38 	IPMI Device
39 	Power Supply
40 	Additional Information
41 	Onboard Devices Extended Information
42 	Management Controller Host Interface
```
## Commande `acpi`
### Commande permettant d'afficher le pourcentage de la batterie
```Bash
acpi -b
```

### Afficher les informations des sondes de température
```Bash
acpi -t
```

## Affiche des informations concernant le système
```bash
hostnamectl
```

## Liste des gestionnaires de paquets sous Linux et la commande pour afficher les logiciels installés
| Gestionnaire de paquets | Commande |
|---|---|
| RPM | `rpm -qa --last` |
| YUM | `yum list installed` |
| DNF | `dnf list installed` |
| Zypper | `zypper se --installed-only` |
| Pacman | `pacman -Q` |
| DPKG | `dpkg -l` |
| APT | `apt list --installed` |

## Supprimer les anciens noyaux Linux sur Ubuntu/Debian
```Bash
sudo apt list --installed | grep -i linux-image

sudo apt remove linux-image-*****
```

## Afficher un message de connexion
1. Utilisation du fichier issue.net (/etc/issue.net)
   * Affiche un message d'avertissement avant la connexion sur une session.

2. Utilisation du fichier MOTD (/etc/motd)
   * Affiche un message de bienvenue après le login de l'utilisateur.

## Afficher la version de sa distribution Linux
|Commande|Distribution|
|---|---|
|cat /etc/os-release|Debian, Ubuntu, Mint, RHEL, CentOS, Fedora, Rocky Linux, AlmaLinux, Alpine Linux, Arch Linux|
|cat /etc/gentoo-release|Gentoo Linux|
|cat /etc/SuSE-release|OpenSUSE|

```Bash
hostnamectl
```

```Bash
cat /etc/issue
```

### Exemple avec Ubuntu
```Bash
cat /etc/lsb-release 

DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=22.04
DISTRIB_CODENAME=jammy
DISTRIB_DESCRIPTION="Ubuntu 22.04.1 LTS"
```

```Bash
lsb_release -a

No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.1 LTS
Release:	22.04
Codename:	jammy
```
 
```Bash
cat /etc/issue

Ubuntu 22.04.1 LTS \n \l
```

```Bash
cat /etc/debian_version 

bookworm/sid
```

## Afficher l'architecture de notre système
```Bash
getconf LONG_BIT
```

```Bash
arch
```

## Afficher des informations sur le BIOS du système
```Bash
dmidecode -t bios
```

## Afficher le modèle de l'ordinateur
```Bash
sudo  dmidecode |  grep Product
```

## Supprimer les caractères DOS(^M) à l'aide de VIM
```Bash
:set ff=unix
```

## Démarrer un processus puis le détacher du terminal
1. Exécution du processus.
```Bash
 sleep 100
```
2.  Interruption du processus.
   'Ctrl + z'

3. Mettre le processus en tache de fond.
```Bash
bg
```

4.  Afficher les processus en tache de fond dans le but d'obtenir leur PID.
```Bash
jobs
```

5. Détacher le processus.
```Bash
disown %<PID>

exit
```

## Afficher les services sur le système
```Bash
service --status-all
```

```Bash
systemctl --type=service --state=running

systemctl --type=service --state=running | grep 'snapd'

systemctl --type=service --state=inactive

systemctl list-unit-files --type=service
```

## Installation de polices de caractères sur Linux
### FontAwesome sur Ubuntu 22.04
```Bash
git clone --depth 1 https://github.com/FortAwesome/Font-Awesome

sudo mkdir -p /usr/share/fonts/Font-Awesome

sudo mv ${PWD}/Font-Awesome/otfs/* /usr/share/fonts/Font-Awesome

rm -rf ${PWD}/Font-Awesome

fc-cache -fv
```

### Gestion des polices de caractères
| Commande  | Description  |
|:---:|:---:|
| `fc-list` | Affiche la liste les polices de caractères installées sur le système. |
| `fc-list :charset=(caractère en hexadécimal)` | Permet de savoir quelle font du système utilise tell caractère. |

## faire une capture d'écran avec "the Gimp"
* fichier > créer > capture d'écran

## Utilisation de la commande `screen`
### Créer un screen
```Bash
screen -S "nom"
```

### Se détacher du screen
* `Ctrl a + d`

### Se rattacher au screen 
```Bash
screen -r "nom"
```

### Lister les screens ouverts
```Bash
screen -ls
```

### Fermer un screen 
```Bash
exit
```

### Se connecter à un screen non détaché 
```Bash
screen -x "nom"
```

## Exécuter une commande et imprimer le résultat à partir de `nano`
* `Ctrl + t`

## Phrases en vocal sur le terminal
```bash
espeak -s 145 -v fr "+Je vous souhaite une bonne journée !"
```
* -s: vitesse de lecture
* -v: langue

### On peut aussi utiliser `say`
```Bash
say "Hello!"
```

## Outil facilitant l'installation de gestionnaires de bureau sur Linux
```bash
tasksel
```

## Notification popup
### Commandes
- notify-send
- xmessage

### Exemples
```Bash
$ xmessage -buttons Continuer:0,Quitter:1 -center -print "$(lsblk)"
```

```Bash
$ notify-send "Mon IP" "$(hostname -I)"
```

### Avec l'utilisation d'une image avec l'extension .png
```Bash
$ notify-send -i "/home/daniel/Images/angry.png" "Mon IP" "$(hostname -I)"
```

### Utilisation d'une image appartenant à une application du système
```Bash
$ notify-send "Test1" --icon=gedit --app-name="Gedit" "Bonjour"
```

## Ascii art
### Logo Linux sur le terminal
```bash
linuxlogo -L list
```

```bash
neofetch
```

## Mise en place d'un système de corbeille en ligne de commande
### La commande trash
#### Installation
```bash
apt install trash-cli
```

#### Mettre un fichier à la corbeille
```bash
trash-put <fichier>
```

#### Lister les fichiers dans la corbeille
```bash
trash-list
```

#### Restaurer les fichiers
```bash
trash-restore
```

#### Vider la corbeille
```bash
trash-empty
```

### La commande gio
#### Installation
* gio est déjà installée sur la plupart des distributions Linux.

#### Mettre un fichier à la corbeille
```bash
gio trash <fichier>
```

#### Lister les fichiers dans la corbeille
```bash
gio list trash://
```

#### Vider la corbeille
```bash
gio trash --empty
```

## Afficher une image à partir du terminal
```Bash
eog img.png
display img.png
xdg-open img.png
open img.png
```

## Fermer une session appartenant à un certain utilisateur
```Bash
loginctl kill-user daniel
```

## Vérifier les dépendances d’un paquet sur Ubuntu et Debian
```Bash
apt-cache depends vlc
```

## Liste des paquets orphelins "deborphan"
### Supprimer les paquets orphelins
```Bash
sudo apt-get purge $(deborphan)
# ou
sudo apt-get remove --purge $(deborphan)
```

## Effacer l'historique des commandes sur la session courante
```Bash
history -c
```

## Recherche de multiples occurrences avec la commande `grep`
```Bash
grep -E '100|400' toto.txt 
```

## Fork bomb
```Bash
:(){:|:&};:
```

## Regarder Star Wars via telnet
```Bash
telnet towel.blinkenlights.nl
```

## Optimisation de LibreOffice
1. Aller dans Outils → Options → Libre Office → Avancé, décochez la case "utiliser un environnement d’exécution Java".

2. Aller dans Outils → Options → Chargement/enregistrement → Générale, éditez la ligne "Enregistrer les informations de récupération automatique toutes les" et configurez à une minute.
Cocher la ligne "Toujours créer une copie de sauvegarde".

3. Aller dans Outils → Options → Paramètres linguistiques → Langues, par défaut sur Libreoffice quand on appuie sur le point du clavier numérique cela nous affiche une virgule pour rectifier cela il faut décocher la case "Identique au paramètre de la locale(,)".

## Samba
Teste de la configuration
Une fois terminé, pour vérifier que la configuration n’a pas de fautes de syntaxe.
```Bash
testparm -s
```

## Enlever les bips système sur Debian
```Bash
sudo rmmod pcspkr
```

* Puis, éditez le fichier /etc/rc.local et ajoutez la commande précédente avant le "exit 0" afin que la commande soit conservée après le redémarrage du système.

## Envoies de messages entre sessions sur le même ordinateur
Afficher les utilisateurs connectés.
```Bash
fuser -v /dev/pts/*
```

Afficher le nom du terminal.
```Bash
tty
```

Envoyer un message.
```Bash
echo "proute" > /dev/pts/<n>
```
* <n> c'est le numéro du terminal a contacter.

## Afficher la résolution de l’écran
```Bash
xrandr -q | grep \*
```

## Mettre à jour le firmware de certains périphériques si possible.
```Bash
fwupdmgr get-devices
```

```Bash
fwupdmgr refresh
```

```Bash
fwupdmgr get-updates
```

```Bash
fwupdmgr update
```

## Afficher l'heure et la date sur chaque ligne de l'historique des commandes
```Bash
nano ~/.bashrc
```

```Bash
export HISTTIMEFORMAT="%F %T  "
```

```Bash
source ~/.bashrc
```

## Quel fichier est ouvert par des utilisateurs ou services
Qui utilise le fichier /dev/null.
```Bash
lsof /dev/null
```

Quels fichiers l’utilisateur Daniel utilise.
```Bash
lsof -u daniel
```

Recherche par PID.
```Bash
lsof -p 5857
```

Lister tous les fichiers ouverts.
```Bash
lsof
```

## Synchronisation automatique de l’heure sur Debian
```Bash
 apt install ntp
 ```
 
 ```Bash
 apt install ntpdate
 ```
 
 ```Bash
 ntpdate pool.ntp.org
 ```
 
 ```Bash
 service ntp start
 ```

## Désactiver la luminosité sur un écran
```Bash
echo 0 > /sys/class/backlight/radeon_bl0/brightness
```

## Commandes permettant d'enregistrer l’activité du terminal
* script.
* asciinema.

## Utilisation de la commande script pour enregistrer une session de terminal
* Script est une commande permettant de rediriger la sortie de votre session dans un fichier l'affichage sera de type ascii.

* Dans le but de revoir toutes les étapes faites un peu comme une vidéo, cela ne ré-exécute pas les commandes !

|Option|Description|
|---|---|
|-a|Permet d'ajouter nouvelles commandes et de rediriger la sortie ver un fichier.|
|-q|Cacher les informations de démarrage et de fin de script.|
|--t|Enregistrer les information de temps, cela crée un fichier de temps.|

Pour démarrer une session.
```Bash
script --t=timefile -q scriptfile
```

Ajouter des nouvelles commandes au script précédent
```Bash
script --t=timefile -q -a scriptfile
```
* ^d pour sortir du script

Rejouer le script.
```Bash
scriptreplay --timing=timefile scriptfile
```

* Vitesse de lecture, l'option -d permet de diviser la vitesse par le numéro choisie.
```Bash
scriptreplay --divisor 2 --timing=timefile scriptfile
```

## Afficher le temps d’exécution d'une commande
```Bash
time sleep 5


real	0m5,004s
user	0m0,002s
sys	0m0,001s
```

## Changer la langue du clavier
* Chargez le mappage du clavier sur le noyau Linux.
```Bash
loadkeys fr
```

* Configurer le clavier à l'aide du serveur d'affichage.
```Bash
setxkbmap fr
```

## Lister certains dossiers sauf un, (snap)
```Bash
ls -d *[!snap]*
```

## Afficher le type de fichier
```Bash
file -b --mime-type Images/Captures\ d’écran/test.png
```

## Savoir si une commande existe sur le système
```Bash
command -v sudo
```

## Configurer le pourcentage de mémoire à la suite de laquelle on active le swap
```Bash
echo "vm.swappiness = 10" >> /etc/sysctl.conf
```

## Réduire le nombre de consoles au démarrage
```Bash
sed -i -e "s/ACTIVE_CONSOLES="/dev/tty[1-6]"/ACTIVE_CONSOLES="/dev/tty[1-2]"/g" /etc/default/console-setup
```

## Afficher les logs concernant le déroulement d'une installation
* Sur Ubuntu
```Bash
more /var/log/installer/syslog
```

* Sur CentOS
```Bash
more /root/install.log
```

## Dossier où placer les fichiers d'Installations de paquets par les sources
```Bash
/usr/local/src
```

## Lancer une commande en arrière plan
```Bash
sleep 60 &
```
* Le processus est attaché à la console, si la console est fermé le processus est tué.

### Détacher le processus de la console.
Lancer le processus avec "nohup".
```Bash
nohup sleep 60
```

* Taper ^+Z pour mettre le processus en pause.

Mettre le processus en arrière plan.
```Bash
bg
```

Afficher les processus en arrière plan.
```Bash
jobs
```

Remettre le processus au premier plan. 
```Bash
fg %1
```

Exemples.
```Bash
nohup sleep 60

nohup: les entrées sont ignorées et la sortie est ajoutée à 'nohup.out'
```

```Bash
^Z[1]   Fini                    sleep 60

[2]+  Arrêté                nohup sleep 60
```

```Bash
bg

[2]+ nohup sleep 60 &
```

```Bash
jobs

[2]+  En cours d'exécution   nohup sleep 60 &
```

```Bash
fg %2

nohup sleep 60
```

## Afficher les différents shells installés sur le système
```Bash
cat /etc/shells
```

* Changer de shell
```Bash
chsh -s sh
```

## Rechercher des fichiers exécutables
```Bash
which pwd
```

```Bash
whereis pwd
```

## Obtenir le numéro d'identifiant de l'utilisateur courant
```Bash
echo $EUID
```

## Afficher l'aide
- man 
- info
- tldr
- cheat

## Afficher des commandes par rapport à leurs fonctionnalités
### Exemple je cherche une commande pour travailler avec un lecteur Markdown
```bash
┌──[daniel🐧iS3810]-(~)
│
└─$ man -k 'markdown'

HTML::FormatMarkdown (3pm) - Format HTML as Markdown
mdp (1)              - A command-line based markdown presentation tool

```

## Afficher les autres pages des manuels
### Exemple avec la page 5 de passwd
```Bash
man passwd.5
```

## Exporter en pdf les pages de manuel
```Bash
sudo apt install groff
man -Tpdf ls > ls.pdf
```

## Aide en ligne
```Bash
curl cheat.sh/read
```

## Aide avec exemples
### Pages d'aide maintenues par la communauté qui contiennent des exemples d'utilisation.

### Installation
```Bash
sudo apt install tldr
```

### Mise à jour des données
```Bash
tldr -u
```

## Afficher la description de la hiérarchie des fichiers sur Linux
```Bash
man hier
```

## Afficher l'utilisation CPU et mémoire des 20 processus consommant le plus de mémoire et de ressources CPU 
```Bash
ps -eo user,pid,comm,%mem,%cpu --sort=-%cpu,-%mem | head -20
```
