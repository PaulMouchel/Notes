# Gestion des environnements pour une application Next.js hebergée sur Vercel

It has something to do with the next.config.js.

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

3. La branche est désormails accessible depuis le code
`console.log(process.env.GIT_BRANCH);`

A suivre...