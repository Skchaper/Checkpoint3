<div align="center"><H1> Checkpoint3_Exercice2 </H1></div>

# Exercice 2 : Manipulations pratiques sur VM Linux (temps estimé : 2h30)

## Partie 1 : Gestion des utilisateurs

**Q.2.1.1** Sur le serveur, créer un compte pour ton usage personnel.  

Création de l'utilisateur :  
```
adduser skchaper
```

Renseigner les informations demandées dont le mot de passe.

Ajouter le nouvel utilisateur au groupe sudo pour lui donner les droits administrateur :  
```
usermod -aG sudo skchaper
```

**Q.2.1.2** Quelles préconisations proposes-tu concernant ce compte ?

?????


## Partie 2 : Configuration de SSH
Un serveur SSH est lancé sur le port par défaut.
Il est possible de s'y connecter avec n'importe quel compte, y compris le compte root.

**Q.2.2.1** Désactiver complètement l'accès à distance de l'utilisateur root.

Editer le fichier sshd_config à l'emplacement /etc/ssh/sshd_config :
```
nano /etc/ssh/sshd_config
```

Trouver la ligne PermitRootLogin et indiquer le texte suivant :
```
PermitRootLogin no #disabled
```

![vmware_icpbjcdQ5T.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_icpbjcdQ5T.png)

**Q.2.2.2** Autoriser l'accès à distance à ton compte personnel uniquement.

Editer le fichier sshd_config à l'emplacement /etc/ssh/sshd_config :  
```
nano /etc/ssh/sshd_config
```

Ajouter la ligne suivante :  
```
AllowUsers skchaper
```
  
![vmware_jyIpp3bT2A.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_jyIpp3bT2A.png)

**Q.2.2.3** Mettre en place une authentification par clé valide et désactiver l'authentification par mot de passe

Editer le fichier sshd_config à l'emplacement /etc/ssh/sshd_config :  
```
nano /etc/ssh/sshd_config
```

Pour désactiver l'authentification par mot de passe, trouver et modifier les lignes suivantes :  
```
PasswordAuthentification no
ChallengeResponseAuthentication no
UsePAM no
```

![vmware_Sxl1S2gVUm.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_Sxl1S2gVUm.png)

Pour activer l'authentification par clef ssh, trouver et modifier la ligne suivante :  

```
PubkeyAuthentification yes
```

![vmware_ww0uAT8CJ6.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_ww0uAT8CJ6.png)

Génération d'une clé RSA de 4096 bits :  
```
ssh-keygen -b 4096
```

![vmware_3oPT0OGfqy.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_3oPT0OGfqy.png)

Les clés apparaissent dans le répertoire ".ssh" de l'utilisateur :  

![vmware_4zyBza72Oh.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_4zyBza72Oh.png)

Copier la clé publique pour activer le SSH sans mot de passe (Méthode : commande ssh-copy-id) :  

```
ssh-copy-id utilisateur_distant@adresse_IP_distante
```

Redémarrage du service ssh :  

```
sudo systemctl restart sshd
```

Tester la connexion ssh sans mot de passe :  

```
ssh utilisateur_distant@adresse_IP_distante
```

## Partie 3 : Analyse du stockage

**Q.2.3.1** Quels sont les systèmes de fichiers actuellement montés ?  

Consulter les informations dans le fichier /etc/fstab :  

```
nano /etc/fstab
```

Le disque **/dev/sda1** utilise un système de fichier **linux_**.  

La partition **md0p1** utilise un système de fichier **ext2**.  

La partition **md0p5** utilise un système de fichier **LVM2_m**.  

La partition **cp3--vg-root** sous md0p5 utilise un système de fichier **ext4**.  

![vmware_iwmYjILTRv.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_iwmYjILTRv.png)

La commande ```lsblk -f``` permet également de vérifier ces informations :  

![vmware_xmvIrYwTlG.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_xmvIrYwTlG.png)

**Q.2.3.2** Quel type de système de stockage ils utilisent ?

Consulter les informations sur le type de système de stockage :  
```
fdisk -l
```

**/dev/sda1** utilise le type de système de stockage **RAID Linux**.  
**md0p1** utilise le type de système de stockage **Linux**.  
**md0p5** utilise le type de système de stockage **LVM Linux**.  

![vmware_sjSNNrcg0E.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_sjSNNrcg0E.png)

**Q.2.3.3** Ajouter un nouveau disque de 8,00 Gio au serveur et réparer le volume RAID

Vérification de l'ajout d'un nouveau disque de 8,00 Gio, taper la commande suivante :  

```
lsblk
```

![vmware_warWnkXyLJ.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_warWnkXyLJ.png)

**Réparer le volume RAID :**  

Partitionnement du nouveau disque **sdb** :  

```
fdisk /dev/sdb
```

Ajouter une nouvelle partition (avec n) et créer une partition primaire (avec p) qui prend la totalité du disque, certaines valeurs sont laissées par défaut. 

![vmware_Q0HphtnNvo.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_Q0HphtnNvo.png)

Toujours dans l'utilitaire, modifier le type de la partition (avec t) en RAID Linux auto. Ensuite, taper **L** pour obtenir la liste des différents types de partition, dans le cas d'une partition de type RAID Linux auto, taper **fd** :  

![vmware_4SIwHiMqzJ.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_4SIwHiMqzJ.png)

Enfin taper **w** pour quitter et sauvegarder les modifications. Avec la commande **lsblk** vérifier que le disque est bien partitionné :  

![vmware_3wksxgL12c.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_3wksxgL12c.png)

Vérifier l'état du RAID, il faut aller voir le contenu du fichier **mdstat** :  

```
cat /proc/mdstat
```

![vmware_Mw4twHrraM.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_Mw4twHrraM.png)

La commande **mdadm --detail <nom du raid>** permet d'obtenir plus de détails concernant l'état du RAID :  

![vmware_BJDAHs1gxe.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_BJDAHs1gxe.png)

Le résultat de la commande donne un RAID 1 actif **md0** avec uniquement la partition **sda1**. Il va falloir y ajouter la nouvelle partition **sdb1**. Pour se faire :  

```
mdadm --manage /dev/md0 --add /dev/sdb1
```

Puis consulter l'avancement dans le fichier **/proc/mdstat** :  

![vmware_6cPAiedBMI.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_6cPAiedBMI.png)

Une fois l'opération terminée, le raid est bien réparé :  

![vmware_mPqNIzBhah.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_mPqNIzBhah.png)


**Q.2.3.4** Ajouter un nouveau volume logique LVM de 2 Gio qui servira à héberger des sauvegardes. Ce volume doit être monté automatiquement à chaque démarrage dans l'emplacement par défaut : /var/lib/bareos/storage.

Vérification de l'ajout d'un nouveau disque de 2,00 Gio, taper la commande suivante :  

```
lsblk
```

![vmware_lOYODT9yEM.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_lOYODT9yEM.png)

Créer la partition **sdc1** sur le disque **sdc** avec la méthode vue précédemment pour le partitionnement du disque **/dev/sdb** en choisissant le type de partition **Linux LVM** :  

![vmware_xwYnvWGOCX.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_xwYnvWGOCX.png)

Initier le volume physique par la commande :

```
pvcreate /dev/sdc1
```

![vmware_2Kt6MlX9D2.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_2Kt6MlX9D2.png)

Créer le groupe de volume, en lui renseignant un nom et le volume physique utilisé :  

```
vgcreate mvg /dev/sdc1
```

![vmware_2oDotDibFx.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_2oDotDibFx.png)

Avec la commande ci-dessous, vérifier l'apparition du groupe de volume :  

```
vgdisplay mvg
```

![vmware_avnneYMp6d.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_avnneYMp6d.png)

Créer le volume logique :  

```
lvcreate -n Vol1 -L 1,9g mvg
```

lvcreate -n <nom du volume logique> -L <taille du volume logique> <nom du groupe de volume>

![vmware_COUn2W4sj3.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_COUn2W4sj3.png)

Monter le volume logique :  

```
mkfs -t ext4 /dev/mvg/Vol1
```

```
mount /dev/mvg/Vol1 /var/lib/bareos/storage
```

![vmware_PkhNpgAxTg.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_PkhNpgAxTg.png)

Modification du fichier de configuration **/etc/fstab** pour le montage automatique du nouveau volume au démarrage dans l'emplacement par défaut : /var/lib/bareos/storage :  

```
nano /etc/fstab
```

et ajouter la ligne : /dev/mvg/Vol1 /var/lib/bareos/storage ext4 defaults 0 0

![vmware_JeButi8Bi1.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/vmware_JeButi8Bi1.png)

**Q.2.3.5** Combien d'espace disponible reste-t-il dans le groupe de volume ?

?????

## Partie 4 : Sauvegardes
Le logiciel bareos est installé sur le serveur.  
Les composants bareos-dir, bareos-sd et bareos-fd sont installés avec une configuration par défaut.  

**Q.2.4.1** Expliquer succinctement les rôles respectifs des 3 composants bareos installés sur la VM.

**Bareos Director :**  
Il est responsable de la planification, du contrôle et du lancement des tâches de sauvegardes. Il contrôle l'ensemble des autres composants. Il est installé sur le serveur en charge de la gestion des sauvegardes.  

**Bareos File Daemon :**  
Ce composant est installé sur chaque machine devant être sauvegardée. Il est en charge de collecter les informations à sauvegarder et de les envoyer au Bareos Storage Daemon.  

**Bareos Storage Daemon :**  
Bareos permet d'effectuer des sauvegardes sur différents types de supports (bandes magnétiques, disques, stockage distant...). L'écriture sur ces supports est effectué par un Storage Daemon.  

## Partie 5 : Filtrage et analyse réseau

**Q.2.5.1** Quelles sont actuellement les règles appliquées sur Netfilter ?  

```
nft list tables
```

![]()


**Q.2.5.2** Quels types de communications sont autorisées ?  

?????

**Q.2.5.3** Quels types sont interdit ?  

?????

**Q.2.5.4** Sur nftables, ajouter les règles nécessaires pour autoriser bareos à communiquer avec les clients bareos potentiellement présents sur l'ensemble des machines du réseau local sur lequel se trouve le serveur.  Rappel : Bareos utilise les ports TCP 9101 à 9103 pour la communication entre ses différents composants.

**Création de table :**  

```
nft add table ip Bareos
```

```
nft list tables
```

![VirtualBoxVM_Epcq3sQrgF.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/VirtualBoxVM_Epcq3sQrgF.png)

La nouvelle table est créée.

**Création de chaînes :**  

```
nft add chain ip Bareos input {type filter hook input priority 0\;}
```

```
nft add chain ip Bareos output {type filter hook output priority 0\;}
```

```
nft list table ip Bareos
```

![VirtualBoxVM_xcZBmbPo4X.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/VirtualBoxVM_xcZBmbPo4X.png)

**Création de la règle :** 

```
nft add rule Bareos input tcp dport 9101 accept
nft add rule Bareos output tcp dport 9101 accept
```

```
nft add rule Bareos input tcp sport 9101 accept
nft add rule Bareos output tcp sport 9101 accept
```

```
nft add rule Bareos input tcp dport 9103 accept
nft add rule Bareos output tcp dport 9103 accept
```

```
nft add rule Bareos input tcp dport 9103 accept
nft add rule Bareos output tcp dport 9103 accept
```

```
nft list table ip Bareos
```

![VirtualBoxVM_DBKBSY2Gqi.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO2/VirtualBoxVM_DBKBSY2Gqi.png)

Supprimer les règles en double :  

```
nft -a list table ip Bareos
```

```
nft delete rule <nom> <output/input> handle <N°>
```

## Partie 6 : Analyse de logs

**Q.2.6.1** Lister les 10 derniers échecs de connexion ayant eu lieu sur le serveur en indiquant pour chacun :  

La date et l'heure de la tentative  
L'adresse IP de la machine ayant fait la tentative  

mm:dd:hh ip

1. 
2. 
3. 
4. 
5. 
6. 
7. 
8. 
9. 
10. 

Rien dans le fichier /var/log/auth.log, rien dans les fichiers logs bareos présents dans le dossier /var/log/bareos/... .
