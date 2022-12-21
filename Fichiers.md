# Actions concernant des fichiers
## Ouvrir un fichier avec la commande cat en affichant les numéros de lignes
```bash
cat -n fichier.txt
```

## Faire un tri numérique sur une colonne en particulier à partir d'un fichier
- Ici on tri sur la troisième colonne qui correspond à l'UID des utilisateurs.
- On fait un tri croissant.
```bash
sort -n -t':' -k 3 /etc/passwd 
```

## Rechercher des fichiers  de manière interactive
```Bash
fzf
```

## Gestionnaire de fichiers en ligne de commande
```Bash
ranger
```

## L'option -i."nom" de la commande sed permet de faire une sauvegarde avant de modifier le contenu d'un fichier
```Bash
sed -i.bak "s/loglevel=3 quiet/loglevel=4" /etc/default/grub
```

## Rechercher des occurrences de textes dans un PDF
```Bash
pdfgrep -F 'tot*' file.pdf
```

### Script Bash pour la recherche d'occurrences sur plusieurs fichiers PDF
```Bash
#!/bin/bash

readonly pdf_dir="/home/daniel/Nextcloud/BOOKS/Informatique/G_LINUX/BASH/*/*.pdf"
readonly search_word="linux"

mapfile -t files_tab < <(ls ${pdf_dir})

for pdf_file in "${files_tab[@]}"
do
  echo -e "\nFichier PDF : $pdf_file"
  pdfgrep -n "$search_word"  "$pdf_file"
done
```

## Afficher la première et la dernière ligne d'un fichier avec awk
```Bash
awk 'NR==1{print} END{print}'
```

### Exemple
```Bash
$ df -h --total | awk 'NR==1{print} END{print}'

Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
total              449G    255G  172G  60% -
```

## Éditer la précédente commande dans un éditeur de texte et l'exécuter
```Bash
fc
```

## Combiner et séparer un fichier en plusieurs parties
### Diviser un fichier en plusieurs parties de 6G
```Bash
truncate -s 6G fichier
```

### Avec la commande split
```Bash
split --bytes=2GB --numeric-suffixes fichier fichier.part
```

### Utiliser "--number=4" si vous souhaitez diviser le fichier en quatre morceaux de taille égale. 
```Bash
split --number=4 --numeric-suffixes file file.part
```

### Pour les combiner, utilisez simplement cat
```Bash
cat fichier.part* > fichier.combiné
```

### Tester le fait que les deux fichiers sont identiques
```Bash
md5sum --binary fichier fichier.combiné
916ffee83602f9f59b00cf0971de299f *fichier
916ffee83602f9f59b00cf0971de299f *fichier.combiné
```

## Ouvrir des fichiers, dossiers ou URLs à partir de la ligne de commande avec la commande xdg-open ou open

### Exemples
```Bash
xdg-open 'https://fr.wikipedia.org'
xdg-open "${HOME}"
xdg-open fichier.pdf
xdg-open /tmp/foobar.png
```

## Visionneur d'images gnome en ligne de commande 
```Bash
eog image.png
```

## Déplacer tous les fichiers sauf un
```Bash
mv !(fichier_à_pas_déplacer) destination/

# Ou 

mv $( ls | grep -v "fichier_à_pas_déplacer") destination/
```

### Pour ignorer plusieurs fichiers ou dossiers
```Bash
mv $(ls --ignore={dossier_à_pas déplacer,fichier_à_pas_déplacer_1,fichier_à_pas_déplacer_2}) destination/
```

## Supprimer tous les fichiers sauf un
```Bash
rm -f !(fichier_à_pas_supprimer)
```

## Mise en place de la compatibilités des images en WebP sur Ubuntu 22.04
```Bash
sudo add-apt-repository ppa:helkaluin/webp-pixbuf-loader
sudo apt install webp-pixbuf-loader
```

## Créer un fichier avec une certaine taille
### Fichier composés d'octets contenant des zéros
```Bash
truncate -s 5M test.txt
```

### Demande au système de réserver un espace mémoire
```Bash
fallocate -l 5K test2.txt
```

### Avec la commande dd
#### Fichier composés avec des 0
```Bash
dd if=/dev/zero of=test3.file bs=5MB count=1
```

#### Fichier composés avec des données aléatoires
```Bash
dd if=/dev/urandom of=dany.txt bs=5MB count=1
```
ou
```Bash
dd if=/dev/random of=dany1.txt bs=5MB count=1
```

### Avec la commande head
#### Fichier composés avec des données aléatoires
```Bash
head -c 5MB /dev/urandom > ostechnix.txt
```

### Fichier composés d'octets contenant des zéros
```Bash
head -c 5MB /dev/zero > ostechnix.txt
```

## Afficher les attributs d'un fichier
* Ce son des paramètres différents des autorisations standards, ils contrôlent les caractéristiques concernant immuabilité , et autres au niveau du système de fichiers.

```Bash
lsattr
```

### Modifier les attributs
```Bash
chattr
```
* Les attributs sont stockés dans l'inode correspondant au fichier.

### Description de certains attributs
|Attribut|Description|
|---|---|
|a|Impossible d'écraser les données existantes dans le fichier, les données seront ajoutées à la fin du fichier.|
|c|Le fichier est automatiquement compressé sur le disque et se décompresse à la lecture.|
|R|La donnée atime ne se met pas à jour.|
|D|Les modifications apportées sont écrites immédiatement sur le disque dur,(synchrone).|
|i|Ne peut pas être modifié ou supprimé.|
|s|Permet d'avoir une suppression sécurisée, les blocs du disque dur contenant les données sont écrasés par des octets contenant des zéros.|
|u|Si le fichier est supprimé cela crée une copie.|

### Exemple, rendre le fichier fichier.txt immuable. 
```Bash
sudo chattr +i fichier.txt
```

## Imprimer en utilisant la commande netcat
```Bash
nc 192.168.1.49 9100 < fichier.txt 
```

* Pour la mise en forme utiliser les commandes `pr` et `fmt`, par contre il faut éteindre l'imprimante pour quelle retire la feuille et il faut faire `ctrl + c` pour que l'impression se termine.

## Copier un fichier en affichant la progression
```Bash
pv ubuntu-22.04.1-desktop-amd64.iso > Documents/ubuntu.iso
```

## Afficher les derniers fichiers consultés, modifié ...
### Modifier les deux derniers jours
```Bash
find /home -type f -mtime 2
```

### Modifier il y à deux minutes
```Bash
find /home -type f -mmin 2
```

### Créé les deux derniers jours
```Bash
find /home -type f -ctime 2
```

### Dernier accès des 2 dernières minutes
```Bash
find /home -type f -amin 2
```

### Dernier accès il y a plus de deux jours
```Bash
find /home -type f -amin +2
```

### Combinaison
```Bash
find /home -type f -amin +2 and -ctime -6
```

## Supprimer les dossiers vides
```Bash
find /home/daniel/Images -type d -empty -delete
```

## Changer les permissions de plusieurs fichiers à la fois, de manière récursive.
```Bash
find /home/daniel -type f -exec chmod 644 {} \;
```

## Liste  tous les fichiers ouverts par une certaine application
```Bash
lsof -c dhcp
```