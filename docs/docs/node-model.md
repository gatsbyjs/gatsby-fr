---
title: Modèle de nœud
description: Documentation expliquant le modèle de nœuds dans la couche de données GraphQL de Gatsby
jsdoc: ["gatsby/src/schema/node-model.js"]
apiCalls: NodeModel
contentsHeading: Les méthodes
---

Gatsby expose son stockage de données interne et ses capacités de requête aux résolveurs de champ GraphQL sur `context.nodeModel`.

## Exemple d'utilisation

```javascript:title=gatsby-node.js
createResolvers({
  Requete: {
    ambiance: {
      type: `String`,
      resolve(source, args, context, info) {
        const coffee = context.nodeModel.getAllNodes({ type: `Café` })
        if (!coffee.length) {
          return 😞
        }
        return 😊
      },
    },
  },
})
```
