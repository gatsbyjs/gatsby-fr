---
Titre: Actions
la description: Documentation sur les actions et comment elles vous aident à manipuler l'état dans Gatsby
jsdoc:
  - "gatsby/src/redux/actions/public.js"
  - "gatsby/src/redux/actions/restricted.js"
en-tête du contenu: les fonctions
---

Gatsby utilise [Redux](http://redux.js.org) en interne pour gérer l'état. Lorsque vous implémentez une API Gatsby, vous recevez une collection d'actions (équivalentes aux actions liées avec [bindActionCreators](https://redux.js.org/api/bindactioncreators/) dans Redux) que vous pouvez utiliser pour manipuler l'état sur votre site.

Les `actions` d'objet contiennent les fonctions et celles-ci peuvent être extraites individuellement à l'aide de la déstructuration d'objet ES6.

```javascript
// Pour la fonction createNodeField
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
}
```
