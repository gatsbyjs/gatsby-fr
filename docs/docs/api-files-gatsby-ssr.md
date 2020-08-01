---
title: Le fichier API gatsby-ssr.js
---

Le fichier `gatsby-ssr.js` vous permet de modifier le contenu des fichiers HTML statiques car ils sont rendus côté serveur (SSR) par Gatsby et Node.js. Pour utiliser les [APIs Gatsby SSR](/docs/ssr-apis/), créez un fichier appelé `gatsby-ssr.js` à la racine de votre site. Exportez l'une des APIs que vous souhaitez utiliser dans ce fichier.

Les APIs `wrapPageElement` et `wrapRootElement` existent à la fois dans le SSR et dans les [APIs des navigateurs](/docs/browser-apis). Si vous en utilisez une, demandez-vous si vous devez l'implémenter à la fois dans `gatsby-ssr.js` et `gatsby-browser.js` afin que les pages générées à travers SSR avec Node.js soient les mêmes après avoir été [hydratées](/docs/glossary#hydration) par le moteur JavaScript.

```jsx:title=gatsby-ssr.js
const React = require("react")
const Layout = require("./src/components/layout")

// Adds a class name to the body element
exports.onRenderBody = ({ setBodyAttributes }, pluginOptions) => {
  setBodyAttributes({
    className: "my-body-class",
  })
}

// Wraps every page in a component
exports.wrapPageElement = ({ element, props }) => {
  return <Layout {...props}>{element}</Layout>
}
```
