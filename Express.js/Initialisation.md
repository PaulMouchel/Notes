# Initialisation d'un projet Node.js/Express.js

Executer le commande `npm init -y` (`-y` permet de configurer automatiquement avec tous les parametres par défaut)
=> Le fichier `package.json` est créé

Executer `npm i express` pour installer Express.js
Exectuter `npm install --save-dev nodemon` pour le redémarrage automatique du serveur à chaque modification.

Modifier le fichier `package.json` pour avoir ceci :
```JSON
{
  "name": "example",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "devStart": "nodemon server.js",
    "start": "node server.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  }
}
```

A la racine du répertoire, créer le fichier `server.js`.

A partir de maintenant, on peut executer la commande `npm run devStart` sur le terminal pour executer le code du fichier `server.js`.

# Alternative : Création d'un projet Express.js avec express-draf
Installation globale de express-draft :

    npm i -g express-draft
    
Création du projet Express:

    exp .

## Infos supplémentaires

### utiliser les instruction ES6
Pour utiliser les instruction ES6 `import` et `export`, dans `package.json` :
sous "main", ajouter la ligne :
`		"type": "module",`

### Travailler avec MongoDb

Installer les dépendances suivantes :
`npm i mongoose`
`npm i cors`
`npm i dotenv`

