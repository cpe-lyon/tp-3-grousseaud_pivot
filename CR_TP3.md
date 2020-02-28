# Compte rendu TP3: Gestion des paquets

## Exercice 1:

*1. Quels sont les 5 derniers paquets installés sur votre machine ?*
cat /var/log/dpkg.log | grep " install" |tail-5
tail permet d'afficher les n derniers fichier en partant de la fin (ici n=5)

*2. Utiliser dpkg et apt pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !).*
Avec dpkg: se placer dans var/log/=> dpkg -l | grep "^ii" |wc -l
on selectionne uniquement les paquets commenceant par ii, wc compte, grep agit comme un filtre de recherche,
la liste des paquets dans dpkg commencence par ii.
Avec apt: var/log/ apt list --installed | wc -l
*Comment explique-t-on la (petite) différence de comptage ?*
dpkg:543/apt:544, On observe une différence de comptage car wc comp-te le nbr de ligne or apt possède une ligne("en train de lister...").

*3. Combien de paquets sont disponibles en téléchargement ?*
1er etape lister tout : var/log/ apt list | wc -l
61599
2e etape liste fichiers installés var/log/ apt list --installed | wc -l
549
=> diff=61599-549=61050 fichiers dispo en téléchargement.

*4. Créer un alias “maj” qui met à jour le système*
Se placer dans .bashrc
alias maj="sudo apt full-upgrade"

*5. A quoi sert le paquet fortunes ? Installez-le.*
Les fortunes sont des messages, des citations, des proverbes, etc affichés à chaque connexion dans le terminal.(internet)
Pour l'installer: sudo apt install fortune

*6. Quels paquets proposent de jouer au sudoku ?*
gnome sudoku

*7. Lister les derniers paquets installés explicitement avec la commande apt install*
cat /var/log/dpkg.log | grep "install" |tail -15

## Exercice 2:

*A partir de quel paquet est installée la commande ls ? *Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des
commandes utiles) ?*
dpkg -S $(which -a ls)
Le paquet est installé dans coreutils
*Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension
.sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.*
On écrit un script appelé commande-origine, on écrit : 
#!/bin/bash 
echo $(which -a $1 | xargs dpkg -S)
On sauvegarde avec : sudo chmode +x origine-commande
Pour executer la commande; sudo ./origine-commande ls
Ca donne le paquet ainsi que son emplacement.

## Exercice 3:

*Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package
spécifié dans cette commande.*
On écrit un script avec la commande: 
#!/bin/bash
(dpkg -l $1 | grep "^ii") &&  echo "installé" || echo "non installé"
$1 fait référence à la valeur du premier agrument.

Résultat:
1)sudo ./status fortunes
=>ii fortunes   1:1.99.1-7build1 all    Data files containing fortune cookies
installe
2)sudo ./status vlc
dpkg-query: aucun paquet ne correspond a vlc
non installe

## Exercice 4:

*Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment afficher ce qu’elle
retourne ?*
apt show coreutils
Cela nous retourne: "Précisement, ce paquet comprend : arch base64 basename.....who whoami yes"
La commande '[' est un synonyme pour la commande test. (cf intervention tp)

## Exercice 5:

*Installez le paquet emacs à l’aide de la version graphique d’aptitude.*
sudo apt install aptitude
On lance la version graphique avec: sudo aptitude
On appuie sur / pour effectuer une recherche, on tape emacs puis on l'installe.
Il y a les lettres permattant des raccourcis en haut de page.

## Exercice 6:

*Certains logiciels ne figurent pas dans les dépôts officiels. C’est le cas par exemple de la version ”officielle”
de Java depuis qu’elle est développée par Oracle. Dans ces cas, on peut parfois se tourner vers un ”dépôt
personnel” ou PPA.
1. Installer la version Oracle de Java (avec l’ajout des PPA)
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer
2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?*
On a eu un probleme lors de l'installation, l'install ne marche, on nous propose à la place oracle-java11-installer-local.
L'interface se lance mais il y a des erreurs pendant l'éxecution.



