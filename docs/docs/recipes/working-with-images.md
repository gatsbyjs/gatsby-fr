---
title: "Recettes: travailler avec des images"
tableOfContentsDepth: 1
---

Accédez aux images en tant que ressources statiques ou automatisez le processus d'optimisation grâce à de puissants plugins.

## Importer une image dans un composant avec webpack

Les images peuvent être importées directement dans un module JavaScript avec webpack. Ce processus réduit et copie automatiquement l'image sur votre site `public` dossier, fournir une URL d'image dynamique à transmettre à un code HTML `<img>` élément comme un chemin de fichier normal.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

### Conditions préalables

- Une [Gatsby Site](/docs/quick-start) avec un `.js` fichier exportation d'un composant React
- une image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) dans le `src` dossier

### Directions

1. Importez votre fichier à partir de son chemin dans le `src` dossier:

```jsx:title=src/pages/index.js
import React from "react"
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg" // highlight-line
```

2. Dans `index.js`, ajouter un `<img>` tag avec le `src` comme nom de l'importation que vous avez utilisé à partir de webpack (dans ce cas `FiestaImg`), et ajoutez un `alt` attribut [décrivant l'image](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react"
import FiestaImg from "../assets/fiesta.jpg"

export default () => (
  // The import result is the URL of your image
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. Courir `gatsby develop` pour démarrer le serveur de développement.
4. Affichez votre image dans le navigateur: `http://localhost:8000/`

### Ressources additionnelles

- [Exemple de référentiel d'importation d'une image avec webpack](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [En savoir plus sur toutes les techniques d'image dans Gatsby](/docs/images-and-files/)

## Référencez une image du `static` dossier

Comme alternative à l'importation d'actifs avec webpack, le dossier `static` permet d'accéder au contenu qui est automatiquement copié dans le dossier `public` une fois construit.

C'est un **sortie de secours** pour [cas d'utilisation spécifiques](/docs/static-folder/#when-to-use-the-static-folder), et d'autres méthodes comme [importation avec webpack](#import-an-image-into-a-component-with-webpack) sont recommandés pour tirer parti des optimisations faites par Gatsby.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

### Conditions préalables

- Une [Gatsby Site](/docs/quick-start) avec un `.js` fichier exportation d'un composant React
- Une image (`.jpg`, `.png`, `.gif`, `.svg`, etc.) dans le `static` dossier

### Directions

1. Assurez-vous que l'image se trouve dans votre dossier `static` à la racine du projet. La structure de votre projet pourrait ressembler à ceci:

```text
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. Dans `index.js`, ajoutez une balise `<img>` avec le `src` comme chemin relatif du fichier depuis le dossier `static`, et ajoutez un attribut `alt` [décrivant l'image](https://webaim.org/techniques/alttext/):

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement.
4. Affichez votre image dans le navigateur: `http://localhost:8000/`

### Ressources additionnelles

- [Exemple de référentiel référençant une image du dossier statique](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [Utilisation du dossier statique](/docs/static-folder/)
- [More on all image techniques in Gatsby](/docs/images-and-files/)

## Optimizing and querying local images with gatsby-image

The `gatsby-image` plugin can relieve much of the pain associated with optimizing images in your site.

Gatsby will generate optimized resources which can be queried with GraphQL and passed into Gatsby's image component. This takes care of the heavy lifting including creating several image sizes and loading them at the right time.

### Prerequisites

- The `gatsby-image`, `gatsby-transformer-sharp`, and `gatsby-plugin-sharp` packages installed and added to the plugins array in `gatsby-config`
- [Images sourced](/packages/gatsby-image/#install) in your `gatsby-config` using a plugin like `gatsby-source-filesystem`

### Directions

1. First, import `Img` from `gatsby-image`, as well as `graphql` and `useStaticQuery` from `gatsby`

```jsx
import { useStaticQuery, graphql } from "gatsby" // to query for image data
import Img from "gatsby-image" // to take image data and render it
```

2. Write a query to get image data, and pass the data into the `<Img />` component:

Choose any of the following options or a combination of them.

a. a single image queried by its file [path](/docs/content-and-data/) (Example: `images/corgi.jpg`)

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

b. using a [GraphQL fragment](/docs/using-fragments/), to query for the necessary fields more tersely

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

c. several images from a directory (Example: `images/dogs`) [filtered](/docs/graphql-reference/#filter) by the `extension` and `relativeDirectory` fields, and then mapped into `Img` components

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

**Note**: This method can make it difficult to match images with `alt` text for accessibility. This example uses images with `alt` text included in the filename, like `dog in a party hat.jpg`.

d. an image of a fixed size using the `fixed` field instead of `fluid`

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

e. an image of a fixed size with a `maxWidth`

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

f. an image filling a fluid container with a max width (in pixels) and a higher quality (the default value is 50 i.e. 50%)

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

3. (Optional) Add inline styles to the `<Img />` like you would to other components

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4. (Optional) Force an image into a desired aspect ratio by overriding the `aspectRatio` field returned by the GraphQL query before it is passed into the `<Img />` component

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6, // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. Run `gatsby develop`, to generate images from files in the filesystem (if not done already) and cache them

### Additional resources

- [Example repository illustrating these examples](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [Using Gatsby Image](/docs/using-gatsby-image)
- [More on working with images in Gatsby](/docs/working-with-images/)
- [Free egghead.io videos explaining these steps](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

## Optimizing and querying images in post frontmatter with gatsby-image

For use cases like a featured image in a blog post, you can _still_ use `gatsby-image`. The `Img` component needs processed image data, which can come from a local (or remote) file, including from a URL in the frontmatter of a `.md` or `.mdx` file.

To inline images in markdown (using the `![]()` syntax), consider using a plugin like [`gatsby-remark-images`](/packages/gatsby-remark-images/)

### Prerequisites

- The `gatsby-image`, `gatsby-transformer-sharp`, and `gatsby-plugin-sharp` packages installed and added to the plugins array in `gatsby-config`
- [Images sourced](/packages/gatsby-image/#install) in your `gatsby-config` using a plugin like `gatsby-source-filesystem`
- Markdown files sourced in your `gatsby-config` with image URLs in frontmatter
- [Pages created](/docs/creating-and-modifying-pages/) from Markdown using [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages)

### Directions

1. Verify that the Markdown file has an image URL with a valid path to an image file in your project

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. Verify that a unique identifier (a slug in this example) is passed in context when `createPages` is called in `gatsby-node.js`, which will later be passed into a GraphQL query in the Layout component

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

3. Now, import `Img` from `gatsby-image`, and `graphql` from `gatsby` into the template component, write a [pageQuery](/docs/page-query/) to get image data based on the passed in `slug` and pass that data to the `<Img />` component:

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

4. Run `gatsby develop`, which will generate images for files sourced in the filesystem

### Additional resources

- [Example repository using this recipe](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Featured images with frontmatter](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [Using Gatsby Image](/docs/using-gatsby-image)
- [More on working with images in Gatsby](/docs/working-with-images/)
