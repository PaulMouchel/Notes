# Nom de la branch git dans les variables d'environnement sur Next.JS

1. Installer le package
`npm i current-git-branch --save-dev`

2. Dans `next.config.js`, ajouter le code suivant 
```Javascript
const currentGitBranchName = require("current-git-branch");

module.exports = {
  env: {
    GIT_BRANCH: currentGitBranchName()
  }
};
```

3. La branche est désormais accessible depuis le code
`console.log(process.env.GIT_BRANCH);`
