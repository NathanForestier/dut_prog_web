# TP : Maintenir un état

Durant ces exercices nous allons étudier les moyens permettant de maintenir un état entre deux requêtes. 
Nous étudierons tout d'abord la manière d'ajouter des paramètres pour assurer le bon fonctionnement d'une servlet. Nous mettrons en oeuvre ensuite un "Trains de Servlets" puis des aspects Multi-formulaires. Enfin, nous manipulerons ensuite des champs cachés pour passer des données d'une page à une autre

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

## Manipuler un Cookie

1. Créer une servlet `MonEquipe` qui génère un formulaire permettant de sélectionner son équipe préférée parmis toutes les clubs existants.
1. Après avoir choisi une équipe, la servlet `ListerRencontres` ne doit lister que les rencontres concernant cette équipe. Il doit être possible de fermer complétement le navigateur et de revenir sur cette page, toujours avec le même choix défini. On utilisera pour cela un cookie permettant de stocker le nom de l'équipe choisie.
1. Ajouter un lien vers la servlet `MonEquipe` depuis la servlet `ListerRencontres`. Sur `MonEquipe` ajouter un lien permettant de supprimer le choix d'équipe. Dans ce cas, `ListerRencontres` affiche de nouveau tous les matchs.

## Ajout de champs cachés I

1. Une page peut contenir différents formulaires, mais néanmoins les données de chaque formulaire sont indépendantes. Lorsque l'on valide un formulaire, seules ses données sont transmises à la page indiquée en action. Ajouter un bouton `supprimer` sur chaque ligne, permettant d'effacer la rencontre correspondante. Il faut donc un formulaire par ligne. On passera l'identifiant de la rencontre concernée en champs caché

1. Ajouter un lien "modifier" (tag <a>) à chaque ligne de la page ListeSimple, qui aura pour objectif de permettre de
modifier les données de la rencontre concernée en appelant la servlet suivante.

1. Crééz une servlet `ModifierRencontre` prenant en paramètre le `num_rencontre` pour afficher les données actuelles de
celle-ci dans un formulaire. Un bouton permet de les valider et mettre à jour la table des rencontres. Les données sont
transmises en POST et l'utilisateur est redirigé vers `ListeSimple` après modification des données. 

1. On ne souhaite pas que l'utilisateur puisse  modifier l'identifant de la rencontre. Il faut donc l'enlever de la partie visible du formulaire, mais néanmoins il nous faut cette valeur pour faire la mise à jour. L'usage d'un champs caché permettra de régler ce problème. 


## Multi-formulaire


1. Jusqu'à présent, nous utilisions une servlet à part `SaisirRencontre` pour saisir les nouvelles rencontres. On souhaite maintenant se passer de cette page et permettre la saisie directement depuis la liste. Faites en sorte que le formulaire de saisie apparaisse avant la liste des rencontres sur la page `ListerRencontre`. Il doit donc être possible d'ajouter un match depuis cette page. Lorsque je valide les données du formulaire je souhaite voir la nouvelle rencontre apparaitre dans la liste sans devoir raffraichir la page.


## Ajout de champs cachés II

1. Ajoutez un formulaire contenant 3 champs de saisie placés chacun sous le titre des colonnes `jour`, `club_a` et `club_b` et permettant de filtrer les données de la page. On cherchera donc à construire une clause `Where` contenant la conjonction des différents champs renseignés dans ce formulaire. On ajoutera bien sûr un bouton pour valider les données saisies et envoyer en POST le formulaire.

1. Faites en sorte qu'il soit possible de filtrer selon plusieurs critères et que l'on puisse trier en même temps les données. Pour cela on utilisera systématiquement les valeurs des filtres pour la constitution des URL.
