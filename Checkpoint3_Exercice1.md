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

Kelly Rhameur n'est plus dans l'OU DirectionDesRessourcesHumaines :  
![VirtualBoxVM_4P2MKjhrgm.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_4P2MKjhrgm.png)

Le compte utilisateur de Kelly Rhameur est désactivé et se trouve dans l'OU DeactivatedUser :  
![VirtualBoxVM_L3Ts43gB0t.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_L3Ts43gB0t.png)

**Q.1.1.3** Modifier le groupe de l'OU dans laquelle était Kelly Rhameur en conséquence.

L'utilisateur désactivé de Kelly Rhameur a été retiré du groupe de l'OU et Lionel y a été ajouté à la place :  
![VirtualBoxVM_gVhLh7ot3m.png](https://github.com/Skchaper/Checkpoint3/blob/main/Screens/EXO1/VirtualBoxVM_gVhLh7ot3m.png)

**Q.1.1.4** Créer le dossier Individuel du nouvel utilisateur et archive celui de Kelly Rhameur en le suffixant par -ARCHIVE.


## Partie 2 : Restriction utilisateurs

**Q.1.2.1** Faire en sorte que l'utilisateur Gabriel Ghul ne puisse se connecter que du lundi au vendredi, de 7h à 17h.

**Q.1.2.2** De même, bloquer sa connexion au seul ordinateur CLIENT01.

**Q.1.2.3** Mettre en place une stratégie de mot de passe pour durcir les comptes des utilisateurs de l'OU LabUsers.


## Partie 3 : Lecteurs réseaux

**Q.1.3.1** Créer une GPO Drive-Mount qui monte les lecteurs E: et F: sur les clients.

