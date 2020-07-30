---
title: The gatsby-node.js API file
---

Le code du fichier `gatsby-node.js` est exécuté une fois dans le processus de création de votre site. Vous pouvez l'utiliser pour créer des pages de manière dynamique, ajouter des nœuds dans GraphQL ou répondre à des événements pendant le cycle de vie de la construction. Pour utiliser les [APIs Gatsby Node](/docs/node-apis/), créez un fichier nommé `gatsby-node.js` à la racine de votre site. Exportez l'une des API que vous souhaitez utiliser dans ce fichier.

Chaque API Gatsby Node transmet un [ensemble d'assistants API Node] (/docs/node-api-helpers/). Ceux-ci vous permettent d'accéder à plusieurs méthodes telles que la rapport ou d'effectuer des actions telles que la création de nouvelles pages.

```js:title=gatsby-node.js
const path = require(`path`)
// Log out information after a build is done
exports.onPostBuild = ({ reporter }) => {
  reporter.info(`Your Gatsby site has been built!`)
}
// Create blog pages dynamically
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions
  const blogPostTemplate = path.resolve(`src/templates/blog-post.js`)
  const result = await graphql(`
    query {
      allSamplePages {
        edges {
          node {
            slug
            title
          }
        }
      }
    }
  `)
  result.data.allSamplePages.edges.forEach(edge => {
    createPage({
      path: `${edge.node.slug}`,
      component: blogPostTemplate,
      context: {
        title: edge.node.title,
      },
    })
  })
}
```
