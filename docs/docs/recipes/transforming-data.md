---
title: "Recettes : Transformer les données"
tableOfContentsDepth: 1
---

La transformation des données dans Gatsby est pilotée par des plugins. Les plugins de transformation prennent les données récupérées à l'aide des plugins sources et les transforment en quelque chose de plus utilisable (par exemple, JSON en objets JavaScript, etc.).

## Transformer le Markdown en HTML

Le plugin `gatsby-transformer-remark` peut transformer les fichiers Markdown en HTML.

### Prérequis

- Un site Gatsby avec `gatsby-config.js` et une page `index.js`
- Un fichier Markdown enregistré dans le répertoire `src` de votre site Gatsby
- Un plugin source installé, tel que `gatsby-source-filesystem`
- Le plugin `gatsby-transformer-remark` installé

### Instructions

1. Ajoutez le plugin transformateur dans `gatsby-config.js` :

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. Ajoutez une requête GraphQL au fichier `index.js` de votre site Gatsby pour récupérer les nœuds `MarkdownRemark` :

```jsx:title=src/pages/index.js
export const query = graphql`
  query {
    allMarkdownRemark {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          excerpt
        }
      }
    }
  }
`
```

3. Redémarrez le serveur de développement et ouvrez GraphiQL à `http://localhost:8000/___graphql`. Explorez les champs disponibles sur le nœud `MarkdownRemark`.

### Ressources additionnelles

- [Tutoriel sur la transformation de Markdown en HTML](/tutorial/part-six/#transformer-plugins) en utilisant `gatsby-transformer-remark`
- Consultez les plugins de transformateur disponibles dans la [bibliothèque de plugins Gatsby](/plugins/?=transformer)

## Transformation d'images en niveaux de gris à l'aide de GraphQL

### Prérequis

- Un [site Gatsby](/docs/quick-start) avec un fichier `gatsby-config.js` et une page `index.js`
- Les paquets `gatsby-image`, `gatsby-transformer-sharp`, et `gatsby-plugin-sharp` installés
- Un plugin source installé, tel que `gatsby-source-filesystem`
- Une image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) dans le dossier `src/images`

### Instructions

1. Modifiez votre fichier `gatsby-config.js` pour créer des images et configurez des plugins pour la couche de données GraphQL de Gatsby. Une approche courante consiste à les générer à partir d'un répertoire d'images en utilisant le plugin `gatsby-source-filesystem` :

```javascript:title=gatsby-config.js

 plugins: [
   {
     resolve: `gatsby-source-filesystem`,
     options: {
       name: `images`,
       path: `${__dirname}/src/images`,
     },
   },
   `gatsby-transformer-sharp`,
   `gatsby-plugin-sharp`,
 ],
```

2.  Interrogez votre image en utilisant GraphQL et appliquez une transformation en niveaux de gris à l'image en ligne. Le `relativePath` doit être relatif au chemin que vous avez configuré dans `gatsby-source-filesystem`.

```graphql
  query {
     file(relativePath: { eq: "corgi.jpg" }) {
       childImageSharp {
         // highlight-next-line
         fluid(grayscale: true) {
           ...GatsbyImageSharpFluid
         }
       }
     }
   }
```

Note : Vous pouvez trouver ces paramètres et d'autres dans votre playground GraphQL à l'adresse suivante `http://localhost:8000/__graphql`

3. Ensuite, importez le composant `Img` de "gatsby-image". Vous l'utiliserez dans votre JSX pour afficher l'image.

```jsx:title=src/pages/index.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby"
import Layout from "../components/layout"
// highlight-next-line
import Img from "gatsby-image"

export default () => {
  const data = useStaticQuery(graphql`
    query {
     file(relativePath: { eq: "corgi.jpg" }) {
       childImageSharp {
         // highlight-next-line
         fluid(grayscale: true) {
           ...GatsbyImageSharpFluid
         }
       }
     }
   }
  `)
  return (
    <Layout>
      <h1>I love my corgi!</h1>
      // highlight-start
      <Img
        fluid={data.file.childImageSharp.fluid}
        alt="A corgi smiling happily"
      />
      // highlight-end
    </Layout>
  )
}
```

4. Lancez `gatsby develop` pour démarrer le serveur de développement.

5. Visualisez votre image dans le navigateur : `http://localhost:8000/`

### Ressources additionnelles

- [Documents API, y compris des conseils pour les requêtes en niveaux de gris et en double ton](/docs/gatsby-image/#shared-query-parameters)
- [Documentation Gatsby Image](/docs/gatsby-image/)
- [Exemples de traitement d'images](https://github.com/gatsbyjs/gatsby/tree/master/examples/image-processing)
