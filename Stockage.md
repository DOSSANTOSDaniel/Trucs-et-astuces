# Stockage
## Supprime les données d'un disque entier de manière sécurisé
```bash
nwipe
```

## Affiche l’occupation des disques
### Commandes :
* `df -h`
* `dfc`
* `pydf`

## Test de vitesse d'écriture et lecture d'un disque
```bash
#!/bin/bash
echo -e "\n Vitesse d'écriture"
sync; dd if=/dev/zero of=/tmp/smart bs=1M count=1024; sync

echo -e "\n Vitesse de lecture"
dd if=/tmp/smart of=/dev/null bs=1M count=1024

rm -rf /tmp/smart
```

## Afficher la progression de la copie
```bash
dd if=/dev/zero | pv | dd of=/dev/sdc1  bs=512
```

## Création d'une image disque
```bash
dd if=/dev/sda1 conv=sync,noerror status=progress bs=128K | gzip -c > image.gz
```
### Copie sur un nouveau disque 
```bash 
gunzip -c image.gz | dd of=/dev/sdb1
```
### Création d'une image disque sur une machine distante
```bash 
dd if=/dev/sda1 conv=sync,noerror status=progress bs=128K | gzip -c | ssh daniel@192.168.1.48 dd of=image.gz
```

## Afficher le type de disque, HDD, SSD...
```Bash
sudo fdisk -l /dev/sda | grep -m1 ^Disk | awk '{print $3 " " $4}'
```
Ou
```Bash
cat /sys/class/block/sda/device/model
```

## Activer TRIM pour optimiser les performances d'un SSD
```Bash
sudo systemctl enable fstrim.timer
```

## Réparation d’un disque dur, avec certains secteurs endommagés
* Attention le test ne doit pas être fait sur un disque monté, il faut donc utiliser un live cd.

1. La première étape consiste à identifier le disque.
```Bash
lsblk
```
Ou
```Bash
fdisk -l
```

2. Utiliser la commande badblocks pour rechercher des secteurs défectueux
   * Soit en écriture
```Bash
badblocks -wvs /dev/sdx 
```

   * Soit en lecture
```Bash
badblocks -nvs /dev/sdx
```

3. Si nous trouvons des secteurs défectueux on va les enregistrer dans un fichier
```Bash
badblocks -wvs /dev/sdx > badblocks.txt
```

4. Utiliser la commande fsck ou e2fsck pour tenter de réparer les secteurs défectueux
```Bash
e2fsck -cfpv /dev/sdx  < badblocks.txt
```

* Avec la commande fsck
```Bash
sudo fsck -C -t ext4 -l badblocks.txt /dev/sdxx
```

### Autre solution avec la commande e2fsck
* Comprend badblocks, avec un test de lecture et écriture.
```Bash
e2fsck -ccDFY /dev/sdX
```
* Peut prendre plusieurs jour, selon les ressources de votre ordinateur.

## Afficher le statut d'une configuration RAID logiciel
```Bash
cat /proc/mdstat
```

## Afficher le nom des disques ou le type
```Bash
lsblk -ldn -I 8 -o NAME

sda
```

```Bash
 lsblk -ldn -I 8 -o SIZE,MODEL

447,1G SanDisk SSD PLUS 480 GB
```

## Formater un disque
Nettoyage du disque et création d'une nouvelle table de partitions GPT.
```Bash
wipefs --all /dev/sdX

sgdisk --zap-all /dev/sdX
sgdisk --clear /dev/sdX
sgdisk --verify /dev/sdX
```

Création d'une nouvelle partition.
```Bash
sgdisk --largest-new=1 /dev/sdX
```

Formatage de la partition en FAT32
```Bash
mkfs.fat -F32 /dev/sdX1
```

## Afficher les statistiques sur les entrée, sortie des disques
|Option|Description|
|---|---|
|-x|Pour afficher les statistiques étendues.|
|-N|Affiche les statiques d'un disque donné.|

```Bash
iostat -Nx sda
```

## Graver un fichier ISO
```Bash
dd if="file.iso" of="/dev/sdc" status="progress" conv="fsync"
```
|Option|Description|
|---|---|
|conv="fsync"|Synchronise les données pour une finalisation correcte, s'assure que l'intégralité de l'iso a été écrite sur le périphérique.|
|status="progress"|Affiche l'avancement du transfère, temps restant avant la fin.|

* BS= 512 (default), 4096 (4K), 65536 (64K), 1048576 (1M), 4194304 (4M)
```Bash
  dd if=image.iso | pv | dd of=/dev/sdb bs=4M oflag=sync
```

```Bash
sudo dd if=sdcard_image.img of=/dev/sdb bs=4M conv=fsync status=progress
```

## Afficher le statu d'avancement de la commande dd 
### Avec le signal USR1
```Bash
dd if=/dev/zero of=/dev/null &
```

Récupération du PID du processus
```Bash
echo $!

49484
```

Envoi un message au processus pour qu'il affiche le statut
```Bash
kill -USR1 49484
```

Tuer le processus
```Bash
kill 49484
```

### Avec l'option status=progress 
```Bash
dd if=/dev/zero of=/dev/null status=progress
```

### Avec la commande pv 
```Bash
pv /dev/zero | dd of=/dev/null bs=4M
```

## Commande permettant l'affichage de l’occupation du disque
```bash
ncdu
```
