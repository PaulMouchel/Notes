# Gestion des environnements pour une application React.js hebergée sur Firebase

on a `.env.development` et `.env.production` qui correspondent aux variables d'environnement en mode développement et production.
On veut créer une version staging de notre application en production.

## Marche à suivre

### Configuration

1. on créé le fichier `.env.staging` qui contient les variables d'environnement pour notre site de staging.
2. On installe : `npm install env-cmd`
3. Dans `package.json`, on ajout le script suivant : `"build:staging": "env-cmd -f .env.staging react-scripts build",`
4. Créer un nouveau projet firebase avec un nom du ctyle : nom-de-mon-projet-staging
5. Dans le terminal, tapez `firebase use --add` et selectionner le projet de staging, puis donnez-lui l'alias `staging`

### Déploiement en staging

1. `npm run build:staging`
2. `firebase deploy -P staging`

### Déploiement en production

1. `npm run build`
2. `firebase deploy -P default`

## En cas de soucis

Au moment du deploiement de la version de staging sur firebase, si on a une erreur du type :
`The "path" argument must be of type string. Received type undefined`

Essayer ça :
1. Dans `firebase.json` remplacer :

```JSON
{
  "hosting": {
    "public": "build",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```

par :

```JSON
{
  "hosting": {
    "public": "build",
    "ignore": [
      "firebase.json",
      "**/.*",
      "**/node_modules/**"
    ],
    "rewrites": [
      {
        "source": "**",
        "destination": "/index.html"
      }
    ]
  }
}
```
2. `firebase deploy -P staging`
3. Remettre firebase.json comme avant
4. `firebase deploy -P staging`