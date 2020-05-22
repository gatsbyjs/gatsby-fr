---
title: "Recettes: Sourcing de données"
tableOfContentsDepth: 1
---

Le sourcing de données dans Gatsby est basé sur des plugins; Les plugins sources récupèrent les données de leur source (par exemple le plugin `gatsby-source-filesystem` récupère les données du système de fichiers, le plugin` gatsby-source-wordpress` récupère les données de l'API WordPress, etc.). Vous pouvez également vous procurer les données vous-même.

## Ajout de données à GraphQL

La [couche de données GraphQL](/docs/graphql-concepts/) de Gatsby  utilise des nœuds pour modéliser des blocs de données. Les plugins source Gatsby ajoutent des nœuds source que vous pouvez interroger, mais vous pouvez également créer vous-même des nœuds source. Pour ajouter vous-même des données personnalisées à la couche de données GraphQL, Gatsby fournit des méthodes que vous pouvez exploiter.
Cette recette vous montre comment ajouter des données personnalisées en utilisant `createNode ()`.

### Instructions

1. Dans `gatsby-node.js`, utilisez `sourceNodes ()` et `actions.createNode ()` pour créer et exporter des nœuds afin de pouvoir interroger les données.

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" },
  ]

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon),
      },
    }
    actions.createNode(node)
  })
}
```

2. Lancez `gatsby develop`.

   > _Remarque: After making changes in `gatsby-node.js` you need to re-run `gatsby develop` for the changes to take effect._

3. Recherchez les données (dans GraphiQL ou dans vos composants).

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```

### Ressources additionnelles

- Démonstration d'un exemple d'utilisation du plugin `gatsby-source-filesystem` dans [la cinquième partie du tutoriel](/tutorial/part-five/#source-plugins)
- Rechercher les plugins sources disponibles dans la [bibliothèque Gatsby](/plugins/?=source)
- Comprendre les plugins source en en créant un dans le [tutoriel du plugin source Pixabay](/docs/pixabay-source-plugin-tutorial/)
- [Documentation](/docs/actions/#createNode) de la fonction createNode

## Sourcing de données Markdown pour les articles de blog et les pages avec GraphQL

Vous pouvez générer des données Markdown et utiliser [l'API `createPages`](/docs/actions/#createPage) de Gatsby pour créer des pages de manière dynamique.

Cette recette montre comment créer des pages à partir de fichiers Markdown sur votre système de fichiers local à l'aide de la couche de données GraphQL de Gatsby.

### Conditions préalables

- A [Gatsby site](/docs/quick-start) with a `gatsby-config.js` file
- The [Gatsby CLI](/docs/gatsby-cli) installed
- The [gatsby-source-filesystem plugin](/packages/gatsby-source-filesystem) installed
- The [gatsby-transformer-remark plugin](/packages/gatsby-transformer-remark) installed
- A `gatsby-node.js` file

### Instructions

1. In `gatsby-config.js`, configure `gatsby-transformer-remark` along with `gatsby-source-filesystem` to pull in Markdown files from a source folder. This would be in addition to any previous `gatsby-source-filesystem` entries, such as for images:

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. Add a Markdown post to `src/content`, including frontmatter for the title, date, and path, with some initial content for the body of the post:

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. Start up the development server with `gatsby develop`, navigate to the GraphiQL explorer at `http://localhost:8000/___graphql`, and write a query to get all markdown data:

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Query for all markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. Add the JavaScript code to generate pages from Markdown posts at build time by copying the GraphQL query into `gatsby-node.js` and looping through the results:

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)
  if (result.errors) {
    console.error(result.errors)
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`),
    })
  })
}
```

5. Add a post template in `src/templates`, including a GraphQL query for generating pages dynamically from Markdown content at build time:

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

6. Run `gatsby develop` to restart the development server. View your post in the browser: `http://localhost:8000/my-first-post`

### Ressources additionnelles

- [Tutorial: Programmatically create pages from data](/tutorial/part-seven/)
- [Creating and modifying pages](/docs/creating-and-modifying-pages/)
- [Adding Markdown pages](/docs/adding-markdown-pages/)
- [Guide to creating pages from data programmatically](/docs/programmatically-create-pages-from-data/)
- [Example repo](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown) for this recipe

## Sourcing from WordPress

### Conditions préalables

- Un [site Gatsby](/docs/quick-start/) existant avec un fichier `gatsby-config.js` et` gatsby-node.js`
- Une instance WordPress, auto-hébergée ou sur Wordpress.com

### Instructions

1. Installez le plugin `gatsby-source-wordpress` en exécutant la commande suivante:

```shell
npm install gatsby-source-wordpress --save
```

2. Configurez le plugin en modifiant le fichier `gatsby-config.js` de sorte qu'il comprenne les éléments suivants:

```javascript:title=gatsby-config.js
module.exports = {
  ...
  plugins: [
    {
      resolve: `gatsby-source-wordpress`,
      options: {
        // baseUrl devra être mis à jour avec votre source WordPress
        baseUrl: `wpexample.com`,
        protocol: `https`,
        // est-il hébergé sur wordpress.com ou auto-hébergé?
        hostingWPCOM: false,
        // votre site utilise-t-il le plug-in Advanced Custom Fields?
        useACF: false
      }
    },
  ]
}
```

> **Remarque:** Reportez-vous à la [documentation de plugin gatsby-source-wordpress`](/packages/gatsby-source-wordpress/?=wordpre#how-to-use) pour en savoir plus sur la configuration de vos plugins.

3. Créez un composant de modèle tel que `src/templates/post.js` avec le code suivant dedans:

```jsx:title=post.js
import React, { Component } from "react"
import { graphql } from "gatsby"
import PropTypes from "prop-types"

class Post extends Component {
  render() {
    const post = this.props.data.wordpressPost

    return (
      <>
        <h1>{post.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.content }} />
      </>
    )
  }
}

Post.propTypes = {
  data: PropTypes.object.isRequired,
  edges: PropTypes.array,
}

export default Post

export const pageQuery = graphql`
  query($id: String!) {
    wordpressPost(id: { eq: $id }) {
      title
      content
    }
  }
`
```

4. Créez des pages dynamiques pour vos publications WordPress en collant l'exemple de code suivant dans `gatsby-node.js`:

```javascript:title=gatsby-node.js
const path = require(`path`)
const { slash } = require(`gatsby-core-utils`)

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // interroger le contenu des articles WordPress
  const result = await graphql(`
    query {
      allWordpressPost {
        edges {
          node {
            id
            slug
          }
        }
      }
    }
  `)

  const postTemplate = path.resolve(`./src/templates/post.js`)
  result.data.allWordpressPost.edges.forEach(edge => {
    createPage({
      // `path` sera l'url de la page
      path: edge.node.slug,
      // spécifiez le modèle de composant de votre choix
      component: slash(postTemplate),
      // Dans la requête GraphQL du modèle,'id' sera disponible
      // en tant que variable GraphQL pour rechercher les données de ce message.
      context: {
        id: edge.node.id,
      },
    })
  })
}
```

5. Exécutez `gatsby-develop` pour voir les pages nouvellement générées et naviguer à travers elles.

6. Ouvrez `GraphiQL IDE` sur `http://localhost:8000/__graphql` et ouvrez Docs ou Explorer pour observer les champs interrogeables pour `allWordpressPosts`.

Les pages dynamiques créées ci-dessus dans `gatsby-node.js` ont des chemins uniques pour naviguer vers des publications particulières, en utilisant un composant de modèle pour les publications et un exemple de requête GraphQL pour générer le contenu des publications WordPress.

### Ressources additionnelles

- [Débuter avec WordPress et Gatsby](/blog/2019-04-26-how-to-build-a-blog-with-wordpress-and-gatsby-part-1/)
- Plus sur [Sourcing depuis WordPress](/docs/sourcing-from-wordpress/)
- [Exemple en direct sur l'approvisionnement à partir de WordPress](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-wordpress)

## Obtenir des données de Contentful

### Conditions préalables

- Un [site Gatsby](/docs/quick-start/)
- Un [compte Contentful](https://www.contentful.com/)
- L'[interface de commande Contentful](https://www.npmjs.com/package/contentful-cli) installée

### Instructions

1. Connectez-vous à Contentful avec la CLI et suivez les étapes. Il vous aidera à créer un compte si vous n'en avez pas.

```shell
contentful login
```

2. Créez un nouvel espace si vous n'en avez pas déjà un. Veillez à enregistrer l'ID d'espace qui vous a été attribué à la fin de la commande. Si vous disposez déjà d'un espace Contentful et d'un ID d'espace, vous pouvez ignorer les étapes 2 et 3.

Remarque: pour les nouveaux comptes, vous pouvez remplacer l'espace d'intégration par défaut. Vérifiez pour voir les [espaces inclus avec votre compte](https://app.contentful.com/account/profile/space_memberships).

```shell
contentful space create --name 'Gatsby example'
```

3. Remplissez le nouvel espace avec un exemple de contenu de blog en utilisant le nouvel ID d'espace renvoyé par la commande précédente, à la place de `<ID d'espace>`

```shell
contentful space seed -s '<space ID>' -t blog
```

Par exemple, avec un ID d'espace en place: `contentful space seed -s '22fzx88spbp7' -t blog`

4. Créez un nouveau jeton d'accès pour votre espace. N'oubliez pas ce jeton, car vous en aurez besoin à l'étape 6.

```shell
contentful space accesstoken create -s '<space ID>' --name 'Example token'
```

5. Installez le plugin `gatsby-source-contentful` dans votre site Gatsby:

```shell
npm install --save gatsby-source-contentful
```

6. Editez le fichier `gatsby-config.js` et ajoutez le` gatsby-source-contentful` au tableau `plugins` pour activer le plugin. Vous devriez fortement envisager d'utiliser [variables d'environnement](/docs/variables-environnement/) pour stocker votre ID d'espace et votre jeton à des fins de sécurité.

```javascript:title=gatsby-config.js
plugins: [
   // ajouter au tableau avec tous les autres plugins installés
   // highlight-start
   {


    resolve: `gatsby-source-contentful`,
    options: {
      spaceId: `<space ID>`, // ou process.env.CONTENTFUL_SPACE_ID
      accessToken: `<access token>`, // ou process.env.CONTENTFUL_TOKEN
    },
  },
  // highlight-end
],
```

7. Exécutez `gatsby develop` et assurez-vous que le site a été compilé avec succès.

8. Recherchez des données avec l '[éditeur GraphiQL](/docs/introduction-graphiql/) sur `http://localhost:8000/___graphql`. Le plugin Contentful ajoute plusieurs nouveaux types de nœuds à votre site, y compris tous les types de contenu de votre site Web Contentful. Votre exemple d'espace avec un type de contenu "Blog Post" produit un type de nœud "allContentfulBlogPost" dans GraphQL.

![l'interface graphql, avec un exemple de requête décrit ci-dessous](../images/recipe-sourcing-contentful-graphql.png)

Pour rechercher des titres d'articles de blog à partir de Contentful, utilisez la requête GraphQL suivante:

```graphql
{
  allContentfulBlogPost {
    edges {
      node {
        title
      }
    }
  }
}
```

Les nœuds Contentful incluent également plusieurs champs de métadonnées comme `createdAt` ou` node_locale`.

9. Pour afficher une liste de liens vers les articles du blog, créez un nouveau fichier dans `/src/pages/blog.js`. Cette page affichera tous les messages, triés par date de mise à jour.

```jsx:title=src/pages/blog.js
import React from "react"
import { graphql, Link } from "gatsby"

const BlogPage = ({ data }) => (
  <div>
    <h1>Blog</h1>
    <ul>
      {data.allContentfulBlogPost.edges.map(({ node, index }) => (
        <li key={index}>
          <Link to={`/blog/${node.slug}`}>{node.title}</Link>
        </li>
      ))}
    </ul>
  </div>
)

export default BlogPage

export const query = graphql`
  {
    allContentfulBlogPost(sort: { fields: [updatedAt] }) {
      edges {
        node {
          title
          slug
        }
      }
    }
  }
`
```

Pour continuer à développer votre site Contentful, y compris les pages de détails de publication, consultez le reste des [Gatsby docs](/docs/sourcing-from-contentful/) et Ressources additionnelles ci-dessous.

### Ressources additionnelles

- [Construire un site avec React et Contentful](/blog/2018-1-25-building-a-site-with-react-and-contentful/)
- [En savoir plus sur l'approvisionnement à partir de Contentful](/docs/sourcing-from-contentful/)
- [Contentful source plugin](/packages/gatsby-source-contentful/)
- [Types de champs de texte long renvoyés en tant qu'objets](/packages/gatsby-source-contentful/#a-note-about-longtext-fields)
- [Répertoire d'exemple pour cette recette](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-contentful)

## Extraire des données d'une source externe et créer des pages sans GraphQL

Vous n'avez pas besoin d'utiliser la couche de données GraphQL pour inclure des données dans les pages, [bien qu'il y ait des raisons pour lesquelles vous devriez considérer GraphQL](/docs/why-gatsby-uses-graphql/). Vous pouvez utiliser l'API `createPages` du nœud pour extraire les données non structurées directement dans les sites Gatsby plutôt que via GraphQL et les plugins source.

Dans cette recette, vous allez créer des pages dynamiques à partir des données extraites des [points de terminaison REST de PokéAPI] (https://www.pokeapi.co/). [L'exemple complet] (https://github.com/jlengstorf/gatsby-with-unstructured-data/) peut être trouvé sur GitHub.

### Conditions préalables

- Un site Gatsby avec un fichier `gatsby-node.js`
- [CLI Gatsby] (/ docs / gatsby-cli) installé
- Le package [axios] (https://www.npmjs.com/package/axios) installé via npm

### Instructions

1. Dans `gatsby-node.js`, ajoutez le code JavaScript pour récupérer les données de PokéAPI et créez par programme une page d'index:

```js:title=gatsby-node.js
const axios = require("axios")

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"])

  // Créer une page qui répertorie les Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. Créez un modèle pour afficher Pokémon sur la page d'accueil:

```jsx:title=src/templates/all-pokemon.js
import React from "react"

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Behold, the Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
)
```

3. Exécutez `gatsby develop` pour récupérer les données, construire les pages et démarrer le serveur de développement.
4. Affichez votre page d'accueil dans un navigateur: `http://localhost:8000`

### Ressources additionnelles

- [Full Pokemon data repo](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- Plus d'informations sur l'utilisation de données non structurées dans [Utilisation de Gatsby sans GraphQL](/docs/using-gatsby-without-graphql/)
- Quand et comment [interroger des données avec GraphQL](/docs/graphql-concepts/) pour les sites Gatsby plus complexes

## Sourcing de contenu depuis Drupal

### Conditions préalables

- Un [site Gatsby](/docs/quick-start)
- Un site [Drupal](http://drupal.org)
- Le [JSON: module API](https://www.drupal.org/project/jsonapi) installé et activé sur le site Drupal

### Instructions

1. Installez le plugin `gatsby-source-drupal`.

```shell
npm install --save gatsby-source-drupal
```

2. Editez votre fichier `gatsby-config.js` pour activer le plugin et le configurer.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api`, // optionnel, par défaut à `jsonapi`
      },
    },
  ],
}
```

3. Démarrez le serveur de développement avec `gatsby develop` et ouvrez l'explorateur GraphiQL à` http: // localhost: 8000 / ___ graphql`. Sous l'onglet Explorateur, vous devriez voir de nouveaux types de nœuds, tels que «allBlockBlock» pour les blocs Drupal, et un pour chaque type de contenu dans votre site Drupal. Par exemple, si vous avez un type de contenu «Page», il sera disponible en tant que «allNodePage». Pour interroger tous les nœuds "Page" pour leur titre et corps, utilisez une requête comme:

```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. Pour utiliser vos données Drupal, créez une nouvelle page dans votre site Gatsby à `src/pages/drupal.js`. Cette page répertorie tous les nœuds "Page" Drupal.

_**Remarque:** le schéma GraphQL exact dépendra de la façon dont votre instance Drupal est structurée._

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Drupal pages</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. Avec le serveur de développement en cours d'exécution, vous pouvez afficher la nouvelle page en visitant `http: // localhost: 8000 / drupal`.

### Ressources additionnelles

- [Utilisation de Drupal découplé avec Gatsby](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [En savoir plus sur l'approvisionnement auprès de Drupal](/docs/sourcing-from-drupal)
- [Tutoriel: Créer automatiquement des pages à partir de données](/tutorial/part-seven/)
