# Trucs et astuces sur Windows
## Afficher l'historique des mises à jours
```
wmic qfe list
```

## Afficher les informations sur la connexion wlan
```
netsh wlan show interfaces
```

## Désactiver et activer le Pare-feu de Windows
```
netsh advfirewall set allprofiles state off

netsh advfirewall set allprofiles state on
```

## Installer Windows 11 sans compte Microsoft
1. Lancer l'installation avec le cable rj45 déconnecté.
2. A l'étape de la connexion internet faire :
   1. 'shift + F10' pour ouvrir un terminal.
   2. Taper la commande : 'oobe \bypassnro'.
3. Maintenant l'installation va recommencer mais avec la possibilité de ne pas se connecter à internet.
4. Cliquer sur je n'ai pas internet.
5. Continuer avec la configuration limité.

## Vider le cache DNS de Windows
### En CMD
```
ipconfig \flushdns
```

### En Powershell
```
Clear-DnsClientCache
```

## Créer un fichier compressé ou mètre à jour un fichier compressé
```Powershell
Compress-Archive -Path .\toto.iso -Update -DestinationPath .\Exemple.zip
```

## Déplacer un fichier
```Powershell
Move-Item -Path .\toto.txt -Destination .\os\linux.txt
```

## Afficher les mots de passes wifi
### Afficher les ssid
```Powershell
netsh wlan show profiles
```

### Pour chacun afficher le mot de passe
```Powershell
netsh wlan show profile name=net_wifi key=clear
```

## Modifier le mot de passe de Windows
```Powershell
net user daniel password
```

## Utilisation de Winget pour la gestion des applications
### Recherches d'applications
```Powershell
winget search chrome
```

### Installation d'une application
```
winget install balenaEtcher
```

### Mise à jour d'une application
```
winget upgrade Google.Chrome
```

### Désinstallation d'une application
```
winget uninstall Google.Chrome
```

## transfert de fichiers de Windows vers Linux avec la commande pscp
### Sur la machine sous Windows
1. Télécharger pscp.exe ici : https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
2. Afficher le dossier où Powershell rechercher les fichiers exécutables
```
$env:PATH
```

3. Mettre le fichier pscp.exe dans ce dossier.

### Afficher la version installé
```
pscp –version
```

### Envoyer un fichier 
```
pscp fichier.txt daniel@192.168.1.42:/home/daniel/Documents
```

## Récupérer une partition corrompue
* Vérifie et tente de réparer les erreurs sur le disque.
```
CHKDSK /R
```

## Créer un fichier d’une taille spécifique, en octets
```Powershell
fsutil file createnew file.txt 5000
```

## Déconnecter un utilisateur samba sur windows
* Windows par défaut conserve une connexion aux partages à chaque fois qu’on se connecte  à une autre machine ou à un autre serveur.
* 
* Pour se déconnecter, il faut se déconnecter des partages de la machine distante.

Pour voir les connections en cours.
```
net use
```

Pour supprimer une connexion.
```
net use \\<machine>\<partage> /delete							
```

## Autoriser le ping sur Windows serveur
Windows serveur par défaut n’autorise pas le ping.
```Powershell
netsh advfirewall firewall add rule name="ICMP Allow" protocol="icmpv4:8,any" dir=in action=allow
```

## Afficher le mot de passe wifi
Pour afficher tous les SSID autour de vous.
```Powershell
  netsh wlan show all
```

Afficher le mot de passe en clair.
```Powershell
netsh wlan show profile name="Nom SSID" key=clear
```

## Gérer le pare-feu de Windows avec Powershell
Voir toutes les variables d’environnement.
```Powershell
dir env:
```

Créer une règle basée sur Les groupes de règles prédéfinies.
```Powershell
New-NetFirewallRule –DisplayName "Autoriser le Bureau à distance (RDP)" -Group "Bureau à distance" -Profile Domain -Enabled True -Action Allow
```

Déterminer si le pare-feu est activé ou non.
```Powershell
get-netfirewallprofile | ft name,enabled
```

Activer le pare-feu.
```Powershell
PS C:\WINDOWS\system32> set-netfirewallprofile -profile domain,public,private -enabled true
```
Ou
```Powershell
PS C:\WINDOWS\system32> set-netfirewallprofile -profile * -enabled true
```

Désactiver le pare-feu.
```Powershell
PS C:\WINDOWS\system32> set-netfirewallprofile -profile domain,public,private -enabled false
```

Exemple:
```Powershell
new-netfirewallrule -name "SSH" -displayname "SSH serveur" -profile domain,public,private -enabled true -protocol TCP -localport 22 -Action allow
```

## Nettoyer le fichier Winsys
* Le dossier WinSxS contient des fichiers en rapport avec l’utilisation de Windows Update et
l'installation de nouvelles versions de composants.

* Au bout d’un moment le dossier WinSxS commence a devenir trop volumineux.

On va commencer par analyser la taille réelle du dossier WinSxS, ouvrir le terminal
Powershell en administrateur et tapez:
```Powershell
Dism.exe /Online /Cleanup-Image /AnalyzeComponentStore
```

Nettoyage avec la commande StartComponentCleanup si recommandé
```Powershell
Dism.exe /Online /Cleanup-Image /StartComponentCleanup /resetbase
```
* Cette commande va permettre la suppression de packages inutiles et aussi de compresser
certains composants pour gagner de l’espace.

* L’option “/resetbase” permet de supprimer toutes les versions obsolètes des différents
composants.

## Exécuter un script Powershell
* Coller le script dans C:\WINDOWS\system32.
* Exécuter Powershell en tant qu'administrateur.

Activation de l'autorisation d'exécuter des scripts sur le poste.
```Powershell
set-executionpolicy -executionpolicy unrestricted -Force
```

Exécuter le script.
```Powershell
.\script.ps1
```

Ou

```Powershell
powershell -ExecutionPolicy ByPass -File script.ps1
```

Restreindre l'autorisation d'exécuter des scripts sur le poste
```Powershell
set-executionpolicy -executionpolicy restricted -Force
```

## Commandes utiles Powershell
* Lancer une commande en tant qu'administrateur
```Powershell
powershell Start-Process powershell -Verb runAs
```

* Télécharger un fichier à partir d'un site web
```Powershell
Invoke-WebRequest -Uri https://aka.ms/wsl-kali-linux-new -OutFile Kali-linux.appx -UseBasicParsing
```

```Powershell
curl.exe -L -o ubuntu-2004.appx https://aka.ms/wsl-ubuntu-2004
```

* Renommer un fichier
```Powershell
Rename-Item .\Ubuntu.appx .\Ubuntu.zip
```

* Décompresser un fichier ZIP
```Powershell
Expand-Archive .\Ubuntu.zip .\Ubuntu
```

### Mettre à jour Windows
* PSWindowsUpdate est un ensemble de commandes permettant de gérer le client Windows Update.
* Installation de PSWindowsUpdate.
```Powershell
find-Module PSWindowsUpdate
Install-Module -Name PSWindowsUpdate -Force
Import-Module PSWindowsUpdate
```

* Afficher les commandes possibles.
```Powershell
Get-Command -Module PSWindowsUpdate
```

* Afficher les mises à jours disponibles.
```Powershell
Get-WindowsUpdate
```

* Installation des mises à jours disponibles.
```Powershell
Install-WindowsUpdate -AcceptAll -AutoReboot
```

* Avec la création de logs.
```Powershell
Install-WindowsUpdate -AcceptAll -Install -AutoReboot | Out-File "c:\logs\$(get-date -f dd-MM-yyyy_HH-mm)-WinMSJ.log" -force
```

* Installation d'une mise à jour en particulier.
```Powershell
Get-WindowsUpdate -KBArticleID "KB1234567" -Install
```

* Ne pas installer une mise à jour en particulier.
```Powershell
Install-WindowsUpdate -NotKBArticle "KB1234567" -AcceptAll
```

## Mettre à jour ses drivers en ligne de commande sur Windows
PnPUtil.exe est un outil permettant la mise à jour des drivers.
Ouvrir un terminal et taper pnputil.exe.

Lister les paquets des pilotes.
```Powershell
pnputil \enum-drivers
```

Scanner les composants.
```Powershell
pnputil \scan-devices \async
```

## Récupération du mot de passe wifi sur une machine sous Windows
### Méthode 1
Commande pour ouvrir le centre réseau et partage, (ncpa.cpl), faire un clique droit sur statut, propriétés sans fil, sécurité.

### Méthode 2
```Powershell
netsh wlan show profiles
```

```Powershell
netsh wlan show profile name="Poutine_5" key=clear
```

## Équivalent a grep sur Windows
```Powershell
find <occurrence de recherche>
```

## Forcer les mises à jour des stratégies de sécurité
```Powershell
gpupdate /force
```
