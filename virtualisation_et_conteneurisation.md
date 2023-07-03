# Virtualisation et conteneurisation
## Savoir si on est dans une machine virtuelle ou une machine physique

### Avec la commande dmidecode
```bash
sudo dmidecode -s system-manufacturer
```

### Avec la commande lshw
```bash
sudo lshw -class system | grep fabricant
```

### Avec la commande hostnamectl
```bash
hostnamectl chassis
```

### Avec la commande systemd-detect-virt
```bash
sudo systemd-detect-virt
```

### Avec la commande virt-what
```bash
sudo virt-what
```

### Avec la commande facter
```bash
facter virtual
```

## Supprimer toutes les images et containers d'un coup sur Docker
```bash
docker system prune
```

## Se connecter via ssh sur une machine derrière un réseau NAT sur VirtualBox

Pour créer une règle de transfert de port pour la machine virtuelle invitée nommée "Centos" avec l'adresse IP 10.0.2.15 et le port SSH 22, mappé à l'hôte local sur le port 2522
### Eteindre la vm

### Lister les machines virtuelles
```Bash
vboxmanage list vms
```

### Création d'une règle de port forwarding sur le port 22
* Adresse IP de la machine virtuelle : 10.0.2.15
* Port en écoute sur cette machine virtuelle : 22
```bash
vboxmanage modifyvm "Debian" --natpf1 "SSH,tcp,127.0.0.1,2222,10.0.2.15,22"
```

### Vérification
```Bash
vboxmanage showvminfo "Debian" | grep SSH
```

### Connexion à la machine virtuelle à partir de la machine hôte
```Bash
ssh -p 2222 daniel@127.0.0.1
```

## Convertir un disque vdi au format vmdk
```Bash
vboxmanage internalcommands converttoraw debian.vdi debian.raw
```

```Bash
qemu-img convert -O vmdk debian.raw debian.vmdk
```

```Bash
rm debian.raw
```

## Amorçage d’un disque dur physique ou clé USB sur VirtualBox 
* Cette méthode est appelé  VirtualBox "raw hard disk access."

* Créer une configuration de machine virtuelle, puis au moment de créer le disque virtuel cocher : “ne pas ajouter de disque dur virtuel” puis valider.

Brancher votre disque dur ou clé USB sur l’ordinateur et identifier le support.
```Bash
lsblk
```

Une fois le support identifié, il faut identifier le groupe qui a les droits sur les supports.
```Bash
ls -la /dev/sdX
```

Il faut ajouter l’utilisateur courant au groupe trouvé précédemment. 
```Bash
usermod -a -G <groupe> <utilisateur>
```

Attention pour que cela prenne effet il faut se reconnecter sur sa session ou taper la commande `newgrp`.

Se rendre dans le dossier de VirtualBox.
```Bash
cd /home/<utilisateur>/VirtualBox VMs
```

Création du lien entre un disque physique vers un disque logique.
```Bash
VBoxManage internalcommands createrawvmdk -filename <nom du nouveau disque>.vmdk -rawdisk /dev/sdx -partitions <numéro de partition>
```

Configuration des droits.
```Bash
chown <utilisateur>:<utilisateur> <nom du nouveau disque>.vmdk
```

Allez dans les configuration de la machine virtuelle précédemment créé à l’onglet stockage puis sélectionnez ajouter un disque existant et choisissez le disque vmdk que nous avons créé.
Pour finir démarrer la machine virtuelle.

## Augmenter la taille d’un disque virtuel sur VirtualBox
Sur la machine physique
```Bash
vboxmanage controlvm debian poweroff
```

```Bash
vboxmanage modifymedium /home/daniel/VirtualBox\ VMs/debian/debian-disk.vdi --resize 5500000
```
* La taille doit être exprimée en mégaoctets.

```Bash
vboxmanage startvm debian --type headless
```

```Bash
ssh daniel@192.168.1.22
```

Sur la machine virtuelle.
```Bash
screen -S resize_vm
```

```Bash
df -h
```

```Bash
resize2fs /dev/sda
```

Afficher l’évolution de l’agrandissement sur la machine virtuelle.
```Bash
whatch -n 1 -d df -h	
```

Désactiver le Swap.
```Bash
swapoff /dev/sda5	
```

Tue tous les processus qui utilisent la partition.
```Bash
fuser -k /dev/sd4	
```

## Créer ou ajouter un cluster sur Proxmox
Création du cluster.
```Bash
pvecm create CLUSTERNAME
```

Ajouter un nœud sur le cluster.
```Bash
pvecm add IP-ADDRESS-CLUSTER
```

## Enlever la bannière de souscription sur Proxmox 
```Bash
cd /usr/share/javascript/proxmox-widget-toolkit
```

Faire une sauvegarde du fichier de configuration.
```Bash
cp proxmoxlib.js proxmoxlib.js.save
```

Éditer le fichier.
```Bash
nano proxmoxlib.js
```

* Rechercher l'occurrence "if (data.status !== 'Active') {"
et là remplacer par "if (false) {".

Redémarrer le service proxmox, ne pas oublier de vider le cache des navigateurs web.
```Bash
systemctl restart pveproxy.service
```

## Partager le volume d'un conteneur(Docker)
```Bash
docker run -d  -p 443:443 -v dokuwiki:/var/www/html:z daniel
```

* L'option «z» indique à Docker que le contenu du volume sera partagé entre les conteneurs.
* Docker étiquettera le contenu avec une étiquette de contenu partagé.
* Les étiquettes de volumes partagés permettent à tous les conteneurs de lire / écrire du contenu.
suite à cela on pourra écrire directement sur /var/lib/docker/volume...

## Mise en place d'un partage de fichiers sur VirtualBox
Sur la machine virtuelle, pour ajouter le groupe supplémentaire.
```Bash
usermod -a -G vboxsf <nom_utilisateur>
```

```Bash
 sudo mount -t vboxsf PartageVBox /media/sf_PartageVBox
```

## Préparation des machines virtuelles sur Qemu/KVM
### Debian
```Bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install sudo spice-vdagent spice-webdavd qemu-guest-agent git vim -y
usermod -a -G sudo daniel
reboot
```

### Ubuntu
```Bash
sudo apt update && sudo apt full-upgrade -y
sudo apt install sudo spice-vdagent spice-webdavd qemu-guest-agent git vim -y
```

### Fedora
```Bash
sudo dnf upgrade --refresh --assumeyes
sudo dnf install spice-vdagent spice-webdavd qemu-guest-agent git vim --assumeyes
```

### ArchLinux
```Bash
sudo pacman -Suyy
sudo pacman -S spice-vdagent qemu-guest-agent git vim
```

### FreeBSD
```Bash
# freebsd-update fetch
# freebsd-update install
# pkg update
# pkg install sudo qemu-guest-agent git vim
# visudo

daniel ALL=(ALL) ALL
%wheel ALL=(ALL) ALL

reboot
```
