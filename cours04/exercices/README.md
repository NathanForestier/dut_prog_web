# TP : Maintenir un état

Durant ces exercices nous allons étudier les moyens permettant de maintenir un état entre deux requêtes. 
Nous étudierons tout d'abord la manière j'ajouter des paramètres pour assurer le bon fonctionnement d'une servlet, nous manipulerons ensuite des champs cachés pour passer des données d'une page à une autre.Nous mettrons en oeuvre ensuite un "Trains de Servlets" et nous finirons avec des aspects Multi-formulaires.

## Concaténation de paramètres dans l'URL

On souhaite maintenant offrir à l'utilisateur la possibilité de trier la liste des rencontres sur la colonne qu'il souhaite. Pour y parvenir, nous allons passer le critère de tri en paramètre à la requete HTTP. Copiez `ListeSimple` en une nouvelle servlet `ListeRencontres` que vous allez ensuite modifier.

1. Faites en sorte qu’une requête avec un paramètre `tri=nomcol` permette d’afficher la table triée sur la colonne souhaitée
(un seul critère de tri). Cette page doit pouvoir être appelée avec ou sans paramètres.  
http://localhost:8080/vide/ListeRencontres  
http://localhost:8080/vide/ListeRencontres?tri=jour  
1. Modifiez votre servlet pour que les noms des colonnes du tableau soient maintenant des hyperliens permettant le tri d’un simple clic.
1. Ajoutez le sens de tri comme autre paramètre de manière à ce que, à la manière d’un "toggle button" : chaque clic sur le nom de colonne inverse l’ordre de tri  
http://localhost:8080/vide/ListeRencontres  
http://localhost:8080/vide/ListeRencontres?tri=annee&sens=asc  
En ouvrant 2 navigateurs, vous constaterez que deux utilisateurs différents, peuvent avoir chacun leurs propres critères simultanément.


## Ajout de champs cachés

1. Ajoutez un formulaire contenant un champ de saisie sous le titre des colonnes `jour`, `club_a` et `club_b` permettant de filtrer les données de la page. On ajoutera aussi un bouton pour valider les données saisies et envoyer le formulaire. Les données du formulaire seront envoyés en POST.
1. Il doit être possible de filtrer selon plusieurs critères et l'on doit pouvoir trier en même temps les données.

## Manipuler un Cookie

1. Créer une servlet `MonEquipe` qui permet de sélectionner son équipe préférée parmis toutes les clubs existants.
1. Après avoir choisi une équipe, la servlet `ListerRencontres` ne doit lister que les rencontres concernant cette équipe. Il doit être possible de fermer complétement le navigateur et de revenir sur cette page, toujours avec le même choix défini.
1. Ajouter un lien vers `MonEquipe` depuis `ListerRencontres`. Sur `MonEquipe` ajouter un lien permettant d'effacer le choix, dans ce cas, `ListerRencontres` affiche de nouveau tous les matchs.

## Multi-formulaire

Une page peut contenir différents formulaires, mais néanmoins les données de chaque formulaire sont indépendantes. Lorsque l'on valide un formulaire, seules ses données sont transmises à la page indiquée en action.

1. Faites en sorte que le formulaire de saisie apparaisse avant la liste des rencontres sur la page `ListerRencontre`. Il doit donc être possible d'ajouter un match depuis cette page. Lorsque je valide les données du formulaire je souhaite voir la nouvelle rencontre apparaitre dans la liste sans devoir raffraichir la page.
1. Permettez maintenant de pouvoir modifier une rencontre existante à partir de ce formulaire.
1. Ajouter un bouton `supprimer` sur chaque ligne, permettant d'effacer la rencontre correspondante.

