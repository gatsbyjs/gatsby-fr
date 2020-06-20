---
title: Gatsby Node APIs
description: Documentation sur les API Node utilisées dans le processus de construction de Gatsby pour des utilisations courantes comme la création de pages
jsdoc: ["gatsby/src/utils/api-node-docs.js"]
apiCalls: NodeAPI
---

Gatsby offre aux plugins et aux constructeurs de sites de nombreuses API pour contrôler les données de votre site dans la couche de données GraphQL.

## Async plugins

Si votre plugin effectue des opérations asynchrones (disque I/O, accès à la base de données, appel d'API distante, etc.) vous devez soit renvoyer une promesse, soit utiliser le rappel passé au 3e argument. Gatsby doit savoir quand les plugins sont terminés car certaines API, pour fonctionner correctement, nécessitent que les API précédentes soient complètes en premier. Voir [Débogage Async Lifecycles](/docs/debugging-async-lifecycles/) pour plus d'informations.

```javascript
// Promise API
exports.createPages = () => {
  return new Promise((resolve, reject) => {
    // do async work
  })
}

// Callback API
exports.createPages = (_, pluginOptions, cb) => {
  // do Async work
  cb()
}
```

Si votre plugin ne fait pas de travail asynchrone, vous pouvez simplement retourner directement.

## Usage

Implémentez l'une de ces API en les exportant à partir d'un fichier nommé `gatsby-node.js` à la racine de votre projet.
