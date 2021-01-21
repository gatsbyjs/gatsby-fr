---
title: "Recipes: Working with Images"
tableOfContentsDepth: 1
---

Accédez aux images en tant que ressources statiques ou automatisez le processus de leur optimisation grâce à de puissants plugins.

## Import an image into a component with webpack

Les images peuvent être importées directement dans un module JavaScript avec webpack. Ce processus réduit et copie automatiquement l'image dans le dossier `public` de votre site, en fournissant une URL d'image dynamique que vous pouvez passer à un élément HTML `<img>` comme un chemin de fichier normal.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

### Prerequisites

- Un [Site Gatsby](/docs/quick-start) avec un fichier `.js` exportant un composant React
- une image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) dans le dossier `src`

### Directions

1. Importez votre fichier depuis son chemin dans le dossier `src`:

```jsx:title=src/pages/index.js
import React from "react"
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg" // highlight-line
```

2. Dans `index.js`, ajoutez une balise `<img>` avec le `src` comme nom de l'importation que vous avez utilisé à partir de webpack (dans ce cas `FiestaImg`), et ajoutez un attribut `alt` [describing the image](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react"
import FiestaImg from "../assets/fiesta.jpg"

export default () => (
  // The import result is the URL of your image
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement.
4. Affichez votre image dans le navigateur: `http://localhost:8000/`

### Additional resources

- [Example repo importing an image with webpack](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [More on all image techniques in Gatsby](/docs/images-and-files/)

## Reference an image from the `static` folder

Comme alternative à l'importation d'actifs avec webpack, le dossier `static` permet d'accéder au contenu qui est automatiquement copié dans le dossier `public` une fois construit.

Il s'agit d'une **route d'échappement** pour [cas d'utilisation spécifiques](/docs/dossier-statique/#quand-utiliser-le-dossier-statique), et d'autres méthodes comme [importation avec webpack](#import-an-image-into-a-component-with-webpack) sont recommandés pour tirer parti des optimisations faites par Gatsby.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

### Prerequisites

- Un [Site Gatsby](/docs/quick-start) avec un fichier `.js` exportant un composant React
- Une image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) dans le dossier `static`

### Directions

1. Assurez-vous que l'image se trouve dans votre dossier `static` à la racine du projet. La structure de votre projet peut ressembler à ceci:

```text
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. Dans `index.js`, ajoutez une balise `<img>` avec `src` comme chemin relatif du fichier depuis le dossier `static`, et ajoutez un attribut` alt` [décrivant l'image](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement.
4. Affichez votre image dans le navigateur: `http://localhost:8000/`

### Additional resources

- [Example repo referencing an image from the static folder](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [Using the Static Folder](/docs/static-folder/)
- [More on all image techniques in Gatsby](/docs/images-and-files/)

## Optimizing and querying local images with gatsby-image

Le plugin `gatsby-image` peut soulager une grande partie de la douleur associée à l'optimisation des images de votre site.

Gatsby générera des ressources optimisées qui pourront être interrogées avec GraphQL et transmises au composant image de Gatsby. Cela prend en charge les tâches lourdes, notamment la création de plusieurs tailles d'image et leur chargement au bon moment.

### Prerequisites

- Les packages `gatsby-image`, `gatsby-transformer-sharp` et `gatsby-plugin-sharp` installés et ajoutés au tableau des plugins dans `gatsby-config`
- [Images provenant](/packages/gatsby-image/#install) dans votre `gatsby-config` en utilisant un plugin comme `gatsby-source-filesystem`

### Directions

1. Tout d'abord, importez `Img` depuis `gatsby-image`, ainsi que `graphql` et `useStaticQuery` depuis `gatsby`

```jsx
import { useStaticQuery, graphql } from "gatsby" // to query for image data
import Img from "gatsby-image" // to take image data and render it
```

2. Écrivez une requête pour obtenir des données d'image et passez les données dans le composant `<Img/>`:

Choisissez l'une des options suivantes ou une combinaison d'entre elles.

a. une seule image interrogée par son fichier [chemin](/docs/content-and-data/) (Exemple:`images/corgi.jpg`)

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) { // highlight-line
      childImageSharp {
        fluid {
          base64
          aspectRatio
          src
          srcSet
          sizes
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

b. en utilisant un [fragment GraphQL](/docs/using-fragments/), pour interroger les champs nécessaires de manière plus concise

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid {
          ...GatsbyImageSharpFluid // highlight-line
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

c. plusieurs images d'un répertoire (Exemple: `images / dogs`)[filtered](/docs/graphql-reference/#filter) par les champs `extension` et `relativeDirectory`, puis mappés dans les composants `Img`

```jsx
const data = useStaticQuery(graphql`
  query {
    allFile(
      // highlight-start
      filter: {
        extension: { regex: "/(jpg)|(png)|(jpeg)/" }
        relativeDirectory: { eq: "dogs" }
      }
      // highlight-end
    ) {
      edges {
        node {
          base
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`)

return (
  <div>
    // highlight-start
    {data.allFile.edges.map(image => (
      <Img
        fluid={image.node.childImageSharp.fluid}
        alt={image.node.base.split(".")[0]} // only use section of the file extension with the filename
      />
    ))}
    // highlight-end
  </div>
)
```

**Remarque**: Cette méthode peut rendre difficile la correspondance des images avec le texte `alt` pour l'accessibilité. Cet exemple utilise des images avec le texte `alt` inclus dans le nom de fichier, comme`dog in a party hat.jpg`.

d. une image de taille fixe utilisant le champ `fixed` au lieu de `fluid`

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(width: 250, height: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" />
)
```

e. une image de taille fixe avec un `maxWidth`

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(maxWidth: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" /> // highlight-line
)
```

f. une image remplissant un récipient de fluide avec une largeur maximale (en pixels) et une qualité supérieure (la valeur par défaut est 50 soit 50%)

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 800, quality: 75) { // highlight-line
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

3. (Facultatif) Ajoutez des styles en ligne à `<Img />` comme vous le feriez avec d'autres composants

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4.(Facultatif) Forcer une image dans un rapport hauteur / largeur souhaité en remplaçant le champ `aspectRatio` renvoyé par la requête GraphQL avant qu'il ne soit passé dans le composant `<Img />`

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6, // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. Exécutez `gatsby develop`, pour générer des images à partir de fichiers dans le système de fichiers (si ce n'est déjà fait) et les mettre en cache

### Additional resources

- [Example repository illustrating these examples](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [Using Gatsby Image](/docs/using-gatsby-image)
- [More on working with images in Gatsby](/docs/working-with-images/)
- [Free egghead.io videos explaining these steps](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

## Optimizing and querying images in post frontmatter with gatsby-image

Pour des cas d'utilisation comme une image en vedette dans un article de blog, vous pouvez _toujours_ utiliser `gatsby-image`. Le composant `Img` a besoin de données d'image traitées, qui peuvent provenir d'un fichier local (ou distant), y compris d'une URL dans le frontmatter d'un fichier `.md` ou `.mdx`.

Pour insérer des images dans markdown (en utilisant la syntaxe `![]()`), Pensez à utiliser un plugin comme [`gatsby-remarque-images`](/packages/gatsby-remarque-images/)

### Prerequisites

- Les packages `gatsby-image`, `gatsby-transformer-sharp` et `gatsby-plugin-sharp` installés et ajoutés au tableau des plugins dans `gatsby-config`
- [Images provenant](/packages/gatsby-image/#install) dans votre `gatsby-config` en utilisant un plugin comme` gatsby-source-filesystem`
- Fichiers Markdown provenant de votre `gatsby-config` avec des URL d'image en avant
- [Pages créées](/docs/creation-and-modifying-pages/) depuis Markdown en utilisant [`createPages`] (https://www.gatsbyjs.org/docs/node-apis/#createPages)

### Directions

1. Vérifiez que le fichier Markdown a une URL d'image avec un chemin valide vers un fichier image dans votre projet

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. Vérifiez qu'un identifiant unique (un slug dans cet exemple) est passé en contexte lorsque `createPages` est appelé dans `gatsby-node.js`, qui sera ensuite passé dans une requête GraphQL dans le composant Layout

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query for all markdown

  result.data.allMdx.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/components/markdown-layout.js`),
      // highlight-start
      context: {
        slug: node.fields.slug,
      },
      // highlight-end
    })
  })
}
```

3. Maintenant, importez `Img` de `gatsby-image` et `graphql` de `gatsby` dans le composant de modèle, écrivez une [pageQuery](/docs/page-query/) pour obtenir des données d'image basées sur les dans `slug` et transmettez ces données au composant `<Img/>`:

```jsx:title=markdown-layout.jsx
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Img from "gatsby-image" // highlight-line

export default ({ children, data }) => (
  <main>
    // highlight-start
    <Img
      fluid={data.markdown.frontmatter.image.childImageSharp.fluid}
      alt="A corgi smiling happily"
    />
    // highlight-end
    {children}
  </main>
)

// highlight-start
export const pageQuery = graphql`
  query PostQuery($slug: String) {
    markdown: mdx(fields: { slug: { eq: $slug } }) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
// highlight-end
```

4. Exécutez `gatsby develop`, qui générera des images pour les fichiers provenant du système de fichiers

### Additional resources

- [Example repository using this recipe](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Featured images with frontmatter](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [Using Gatsby Image](/docs/using-gatsby-image)
- [More on working with images in Gatsby](/docs/working-with-images/)
