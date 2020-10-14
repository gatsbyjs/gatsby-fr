---
title: Aides Node API
description: Documentation sur les aides API pour la création de nœuds dans la couche de données GraphQL de Gatsby
jsdoc: ["gatsby/src/utils/api-node-helpers-docs.js"]
apiCalls: NodeAPIHelpers
contentsHeading: Shared helpers
showTopLevelSignatures: true
---

Le premier argument transmis à chacune des [API Gatsby Node](/docs/node-apis/) est un objet contenant un ensemble d'aides. Les aides partagées par toutes les API Gatsby Node sont documentées dans la section [Aides partagées](#shared-helpers).

```javascript
// in gatsby-node.js
exports.createPages = gatsbyNodeHelpers => {
  const { actions, reporter } = gatsbyNodeHelpers
  // use helpers
}
```

La convention commune est de déstructurer les aides dans la liste des arguments :

```javascript
// in gatsby-node.js
exports.createPages = ({ actions, reporter }) => {
  // use helpers
}
```

## Remarque

Certaines API fournissent des aides supplémentaires. Par exemple, `createPages` fournit la fonction `graphql`. Consultez la documentation des API spécifiques dans les [API Gatsby Node](/docs/node-apis/) pour plus de détails.
