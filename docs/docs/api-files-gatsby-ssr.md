---
title: The gatsby-ssr.js API file
---

Le fichier `gatsby-ssr.js` vous permet de modifier le contenu des fichiers HTML statiques car ils sont côté serveur rendus (SSR) par Gatsby et Node.js. Pour utiliser les [API Gatsby SSR](/docs/ssr-apis/), créez un fichier appelé `gatsby-ssr.js` à la racine de votre site. Exportez l'une des API que vous souhaitez utiliser dans ce fichier.

Les API `wrapPageElement` et `wrapRootElement` existent à la fois dans le SSR et [browser API] (/docs/browser-apis). Si vous utilisez un d'entre eux, demandez-vous si vous devez l'implémenter à la fois dans `gatsby-ssr.js` et `gatsby-browser.js` afin que les pages générées à travers SSR avec Node.js soient les mêmes après avoir été [hydratées] (/docs/glossary#hydratation) avec browser JavaScript.

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
