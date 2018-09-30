# TP : Paramètres

## Introduction à l'usage des paramètres

### Utiliser la méthode GET

__Palette__

1. Reprenez la servlet Palette. Nous allons permettre de passer la composante de rouge en paramètre de la servlet.  
http://localhost:8080/vide/Palette?r=8  
`r` est un entier représentant la quantité de rouge, entre 0 et 15 inclus.
L'exemple affichera les 256 variations possibles de couleur lorsque la composante de rouge est à '8'. 

1. Il doit être possible d'appeler votre servlet lorsque le paramètre n'est pas fourni, ou qu'il est vide. Vérifiez également que r est entre 0 et 15. Dans tous ces cas proposez une palette avec 0 en rouge.
    * non fourni : http://localhost:8080/vide/Palette
    * vide : http://localhost:8080/vide/Palette?r=
    * hors limite : http://localhost:8080/vide/Palette?r=99  

1. Ajoutez 16 liens avant le tableau de couleurs permettant de naviguer à travers toutes les palettes possibles.  

__Ascii__

1. Reprenez la servlet Ascii permettant d'afficher dans un tableau des caractères 32 à 255 de cette table des caractères. Ajoutez à cette Servlet un paramètre nbcol permettant d’afficher les codes sur plusieurs colonnes à la demande. http://localhost:8080/vide/Ascii?nbCol=5 affichera par exemple les codes sur 5 colonnes.  

1. Ajoutez quelques contrôle sur ce paramètre. Si il n'est pas présent on prendra la valeur 1 par défaut. Ce paramètre ne devra pas être inférieur à 1 et supérieur à 14.  

1. Avant l'affichage du tableau, affichez quelques liens pointant vers des valeurs différentes de 'nbCol'

### Utiliser la méthode POST

1. Modifiez la servlet pour mettre les valeurs de `nbcol` dans une balise `<SELECT>` html.  

1. Mettez le select dans un formulaire html, utilisant la méthode POST et dont l'action sera cette même servlet. Il faudra aussi un bouton pour valider et envoyer les données du formulaire.  
  
Dans une servlet, que le paramètre soit transmis en GET ou en POST, il peut être récupéré avec la même méthode `getParameter()` de l'objet HttpServletRequest.  
Il ne sera donc pas nécessaire de changer cette partie du code.  

1. A chaque fois que vous valider le formulaire, une nouvelle requête arrive au serveur avec une valeur de nbcol.
Donc, une nouvelle page html est générée et le selecteur est remis sur sa première valeur : 1.  
Vous pouvez essayer de placer l'attribut `selected` sur l'option qui correspond à la valeur du parametre nbcol que vous avez reçu. C'est sans doute plus ergonomique.

## Gestion de rencontres sportives

Une fédération sportive organise un championnat entre des clubs. Afin de gérer efficacement ce travail, elle décide de réaliser une application WEB qui s’appuie sur une simple table qui contiendra au fur et à mesure, l’ensemble des résultats de chaque rencontre.  

Le fichier sql fourni dans le cours précédant contient déjà cette table et quelques lignes à l'intérieur.

table _rencontres_

| colonne   | type | description |
|-----------|------|-------------|
| num_match | int  | identifiant de la rencontre |
| eq1       | int  | identifiant de la première équipe, celle qui reçoit |
| eq2       | int  | identifiant de la deuxième équipe, en déplacement |
| jour      | date | jour de la rencontre |
| sc1       | int  | nombre de but inscrits par l'équipe 1 |
| sc2       | int  | nombre de but inscrits par l'équipe 2 |


### Une servlet simple

1. Ecrire une servlet `ListeSimple` qui affiche l’ensemble des informations de cette table du SGBD dans une table HTML.
Cette page est donc appelée par la requête HTTP : http://localhost:8080/vide/ListeSimple

### Saisir et modifier des données

1. Créer une servlet "CreerRencontre" qui permet de saisir et d'insérer une nouvelle rencontre dans la base. L'affichage du formulaire pourra se faire dans le "doGet" et la gestion de l'insersion dans le "doPost". Si l'insertion s'est bien passée, vous pouvez rediriger l'utilisateur vers ListeSimle.  

1. Ajouter un lien "modifier" sur la page ListeSimple qui permet de Modifier les données d'une rencontre.  

1. Il faut donc une servlet "ModifierRencontre" prenant en paramètre le num_rencontre pour afficher les données actuelle de celle-ci dans un formulaire. Un boutton permet de les valider et mettre à jour la table des rencontres. Les données sont transmises en POST et l'utilisateur est redirigé vers ListeSimple après modification des données.  

1. Observer les points communs entre "CreerRencontre" et "ModifierRencontre". Réaliser une servlet "EditerRencontre" qui permettra d'ajouter ou modifier une rencontre.  
Une fois celà fait, les deux premières servlet ne seront plus nécessaire.



