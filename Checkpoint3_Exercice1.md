<div align="center"><H1> Checkpoint3_Exercice1 </H1></div>

# Exercice 1 : Manipulations pratiques sur VM Windows (temps estimé : 1h30)

## Partie 1 : Gestion des utilisateurs
L'utilisateur Kelly Rhameur a quitté l'entreprise.
Elle est remplacée par Lionel Lemarchand

**Q.1.1.1** Créer l'utilisateur Lionel Lemarchand avec les même attribut de société que Kelly Rhameur.  

Commande :  
```New-ADUser -Name "Lionel.Lemarchand" -GivenName "Lionel" -Surname "Lemarchand" -SamAccountName "Lionel.Lemarchand" -Path "OU=DirectionDesRessourcesHumaines,OU=LabUsers,DC=TSSR,DC=LAN" -AccountPassord(Read-Host -AsSecureString "Mot de passe ?") -ChangePasswordAtLogon $true -Enabled $true```  

![VirtualBoxVM_MeK7BzpLoA.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_MeK7BzpLoA.png)  

Vérification de la création de l'utilisateur :
![VirtualBoxVM_TmuuUwmDZF.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_TmuuUwmDZF.png)

**Q.1.1.2** Créer une OU DeactivatedUsers et déplace le compte désactivé de Kelly Rhameur dedans.

Kelly Rhameur n'est plus dans l'OU DirectionDesRessourcesHumaines (via glisser-déposer, plus propre en PowerShell) :  
![VirtualBoxVM_4P2MKjhrgm.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_4P2MKjhrgm.png)

Le compte utilisateur de Kelly Rhameur est désactivé (Clic droit sur l'utilisateur --> Disable) et se trouve dans l'OU DeactivatedUser :  
![VirtualBoxVM_L3Ts43gB0t.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_L3Ts43gB0t.png)

**Q.1.1.3** Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.

Pour se faire, rendez-vous dans l'OU : DirectionDesRessourcesHumaines. **Clic droit** sur le groupe : GrpUsersDirectionDesRessourcesHumaines puis sélectionner **Properties**. Une fois dans la nouvelle fenêtre, **Clic gauche** sur l'onglet **Members**, puis sur **Add**. Renseigner le nom d'utilisateur de Lionel Lemarchand et valider. Puis **clic droit** sur Kelly Rhameur et enfin **remove**.

L'utilisateur désactivé de Kelly Rhameur a été retiré du groupe de l'OU et Lionel y a été ajouté à la place :  
![VirtualBoxVM_gVhLh7ot3m.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_gVhLh7ot3m.png)

**Q.1.1.4** Créer le dossier Individuel du nouvel utilisateur et archive celui de Kelly Rhameur en le suffixant par -ARCHIVE.

Création du dossier Individuel du nouvel utilisateur :  
- Aller dans le disque DossiersIndividuels (F:), clic droit --> New --> Folder --> Nommer le dossier (ici : lionel.lemarchand)  

Archivage du dossier Individuel de l'ancien utilisateur :  
- Toujours dans le disque DossiersIndividuels (F:), clic droit --> Send to... --> Compressed (zipped) folder  

Le dossier de Lionel Lemarchand est bien existant et le dossier de Kelly Rhameur est bien archivé :  
![VirtualBoxVM_J8yXmacxDy.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_J8yXmacxDy.png)


## Partie 2 : Restriction utilisateurs

**Q.1.2.1** Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.

Tout d'abord, rendez-vous dans le menu propriétés de l'utilisateur Gabriel Guhl. Cliquer sur **Account**, puis sur **Logon Hours...**, un nouveau menu s'ouvre :  
![VirtualBoxVM_qeHvx2lGtx.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_qeHvx2lGtx.png)

Ensuite, il faut définir la plage souhaiter, on sélectionne les heures où la connexion ne sera pas permise et on coche **Logon Denied** :  
![VirtualBoxVM_FV46UyiZcl.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_FV46UyiZcl.png)

Une fois la configuration validée, la connexion de l'utilisateur est bien limitée.


**Q.1.2.2** De même, bloquer sa connexion au seul ordinateur CLIENT01.

Rendez-vous dans le menu propriétés de l'utilisateur Gabriel Guhl. Cliquer sur **Account**, puis sur **Log On To...**, un nouveau menu s'ouvre :
![VirtualBoxVM_pVndSedbyy.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_pVndSedbyy.png)

Ensuite, il faut cocher la case **The following computers**, puis renseigner le nom de l'ordinateur sur lequel autoriser l'utilisateur à se connecter :  
![VirtualBoxVM_PVPtS209s9.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_PVPtS209s9.png)

Une fois validé, l'utilisateur Gabriel Guhl pourra se connecter uniquement à l'ordinateur "CLIENT01".

**Q.1.2.3** Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.

Dans la barre de recherche Windows taper **dsac.exe**, une nouvelle fenêtre s'ouvre :  
![VirtualBoxVM_O3tp68oVcA.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_O3tp68oVcA.png)

Sélectionner le domaine : **TSSR (local)**, puis **System** :  
![VirtualBoxVM_V8lL5j6N41.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_V8lL5j6N41.png)

Et enfin **PasswordSettings Container** :  
![VirtualBoxVM_u8tXSTRwNV.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_u8tXSTRwNV.png)

Cliquer sur **New** puis **Password settings** :  
![VirtualBoxVM_q0EuYjyt58.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_q0EuYjyt58.png)

Définir les paramètres au besoin dans le cas de l'exercice, la configuration est la suivante :    
![VirtualBoxVM_Vz3HQ4pUb6.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_Vz3HQ4pUb6.png)

Une fois validée et configurée, la politique de mot de passe apparaît dans l'onglet **Password Settings Container**.
![VirtualBoxVM_YosKK9aTMO.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_YosKK9aTMO.png)


## Partie 3 : Lecteurs réseaux

**Q.1.3.1** Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.

Dans un premier temps, il faut accéder au **Group Policy Management** disponible dans le menu **Tools** :  
![VirtualBoxVM_RIVgxvD7d1.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_RIVgxvD7d1.png)

Une fois au bon endroit, créer une nouvelle GPO sous **Group Policy Objects** et la nommer pour l'identification de celle-ci :  
![VirtualBoxVM_XPMfkjSvzv.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_XPMfkjSvzv.png)

Pour configurer la GPO, clic droit sur la nouvelle GPO puis clic gauche sur **Edit**, puis parcourir l'arborescence : **User Configuration > Preferences > Windows Settings > Drive Maps** :  
![VirtualBoxVM_eoY1OpqFmv.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_eoY1OpqFmv.png)

Dans le nouveau menu, clic droit : **New > Mapped Drive** :  
![VirtualBoxVM_t4DbMV2Cvk.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_t4DbMV2Cvk.png)



