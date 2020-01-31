TP2
===
_infos pratiques_ : tous les raspberry ont pour login `pi` et pour mot de passe `raspberry`

Lancer le serveur node du TP précédent
----------------------------------------------------
. Connectez vous au raspberry à l'aide de la commande ssh:  

```
$ ssh pi@nom-du-raspberry.local
```  
Le nom du raspberry est sur la boîte en carton dans laquel il se trouvait.  
_Depuis Windows vous pouvez utiliser le logiciel "Putty" pour vous connecter à distance sur le raspberry. Par contre Windows ne reconnais pas les noms, il faudra donc utiliser l'adresse IP du raspberry_

. Récupérer le contenu du tp précédent via Git. (_créez au préalable un dossier pour votre binome dans lequel vous clonerez votre projet, utilisez pour cela les lignes de commandes vues au précéndent TP._)

. Lancez le serveur.  
_Rendez vous sur http://nom-du-raspberry.local:3000/_  
(rappel : Ctrl-C pour quitter une commande linux en cours)

**Pour éditer un fichier sous linux, nous allons utiliser le logiciel "nano", essayez d'ouvrir votre fichier server.js avec :**

```
$ nano server.js
```
(Ctrl-X pour quitter nano)

**Pour la suite nous allons avoir besoin d'un peu plus que quelques phrases retournées par notre serveur, pour cela nous allons utiliser un "moteur de template" : Mustache**  

. Installez mustache & mustache-express 

```
$ npm install mustache mustache-express --save
```

. créez une nouvelle hierarchie de dossier

```
votre_dossier/
	- server.js
	- public/ 
 	- views/ 
```
`server.js` => c'est votre code node.js pour lancer votre serveur.  
`public/` => c'est un dossier qui contient des fichiers accessibles directement depuis le serveur. On y range habituellement les fichiers javascript clients, les feuilles de styles css, et les images.  
`views/` => c'est le dossier où vous allez ranger les fichiers templates (les fichiers qui seront transformés en HTML à l'aide du moteur de template Mustache)  

Normalement vous devez également avoir un dossier `node_modules` et un fichier `package-lock.json`, n'y touchez pas, ils sont nécessaire pour le fonctionnement du serveur.

. créez un fichier `index.mustache` dans le dossier `views/` : 

```html
<html>
	<body>
		<h1>Hello with mustache</h1>
	</body>
</html>
```

et créez également un fichier `hello.mustache` dans le dossier `views/` : 

```html
<html>
	<body>
		<h1>Hello, {{ name }}</h1>
	</body>
</html>
```

. Modifiez votre fichier `server.js` de cette manière (faites des copier/coller uniquement pour les lignes nécessaire, si vous copier coller le contenu du code ci-dessous tel quel, cela ne compilera pas ;-) ): 

```js
const express = require('express')
const app = express()
const port = 3000
+++++++++Lignes à ajouter+++++++++++++
//OS est un utilitaire node qui va nous servir à afficher le nom de notre raspberry
const os = require("os");
//MustacheExpress est notre moteur de template
const mustacheExpress = require('mustache-express');
++++++++++++++++++++++++++++++++++++++

+++++++++Lignes à ajouter+++++++++++++
//Configuration du moteur de template
app.engine('mustache', mustacheExpress());
app.set('view engine', 'mustache');
app.set('views', __dirname + '/views');
++++++++++++++++++++++++++++++++++++++

+++++++++Lignes à ajouter+++++++++++++
//Ici on dit au serveur de servir les fichiers statiques depuis le dossier /public
app.use(express.static('public'))
++++++++++++++++++++++++++++++++++++++


//On retrouve le même comportement que notre serveur précédent
app.get('/', (request, response) => {
+++++++++Lignes à modifier+++++++++++++
  //Ici on indique que nous voulons transformer notre fichier index.mustache en HTML
  response.render('index');
++++++++++++++++++++++++++++++++++++++
})

app.get('/hello/:name', (request, response) => {
+++++++++Lignes à modifier+++++++++++++
  //De la même manière nous transformons notre fichier hello.mustache en HTML en passant des paramètres.
  response.render('hello', {name: request.params.name});
++++++++++++++++++++++++++++++++++++++
})

app.listen(port, (err) => {
  if (err) {
    return console.log('Erreur du serveur : ', err)
  }
+++++++++Lignes à modifier+++++++++++++
  //On utilise l'utilitaire OS pour récupérer le nom de notre raspberry.
  console.log('Le serveur écoute sur le port '+port+'\nRendez vous sur http://'+os.hostname()+'.local:'+port);
++++++++++++++++++++++++++++++++++++++
})
```

. Pour le moment notre serveur ne répond qu'à deux urls (`/`et `/hello/:name`). Créez une troisième "url" `/pooc` qui affiche une page HTML avec un peu de css.

. Sauvegardez votre travail sur Git.

