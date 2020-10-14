---
title: Créer des pages de manière dynamique à partir de données
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ce tutoriel est inclus dans la série sur les couches de données Gatsby. Assurez-vous d'avoir parcouru les tutoriels
 [numéro 4](/tutorial/part-four/), [numéro 5](/tutorial/part-five/), et [numéro 6](/tutorial/part-six/) avant de continuer.

## Que contient ce tutoriel ?

Dans le tutoriel précédent, vous avez créé une belle page d'accueil ("index page") qui récupère des fichiers markdown et affiche une liste de titres et d'extraits d'articles de blogs. Mais vous ne voulez pas uniquement visualiser des extraits: vous souhaitez afficher des pages complètes pour chacun de vos fichiers markdown.

Vous pourriez continuer à créer des pages en plaçant des composants React dans `src/pages`. Toutefois, vous allez maintenant apprendre à créer des pages de manière _dynamique_ à partir de _données_. Contrairement à d'autres générateurs de sites Web statiques, Gatsby ne se limite _pas_ à la création de pages à partir de fichiers. Gatsby vous permet d'utiliser GraphQL pour aller chercher vos _données_ et de _mapper_ les résultats de vos requêtes sur des _pages_ - et tout cela au moment du build. C'est une idée vraiment puissante. Vous allez en explorer les implications et les moyens de s'en servir dans la suite de ce tutoriel.

Commençons.

## Créer des slugs pour les pages

Un 'slug' est une chaîne de caractères qui identifie de façon unique une adresse web. Par exemple, pour la page `https://www.gatsbyjs.org/tutorial/part-seven/`, il s'agit de la chaîne `/tutorial/part-seven`.

Le 'slug' est parfois appelé 'chemin' (ou 'path'), mais, dans ce tutoriel, pour des raisons de cohérence, nous utiliserons le terme 'slug'.

La création de nouvelles pages s'effectue en deux étapes :

1. Créer un 'chemin' ou un 'slug' pour la page.
2. Créer la page en elle-même.

_**Remarque** : Souvent les sources de données vont directement fournir un slug ou un chemin d'accès pour le contenu - quand vous travaillez avec de tels systèmes (par exemple, avec un CMS), vous n'avez pas de besoin de créer les slugs par vous-même. Par contre, vous devez le faire si vous travaillez avec des fichiers markdown._

Pour créer vos pages markdown, vous devez apprendre à vous servir de deux APIs de Gatsby:
[`onCreateNode`](/docs/node-apis/#onCreateNode) et
[`createPages`](/docs/node-apis/#createPages).
Ces deux APIs bien pratiques sont utilisées dans de nombreux sites et plugins.

Nous nous efforçons de faire les APIs de Gatsby faciles à implementer. Pour implementer une API, 
vous devez exporter une fonction portant le nom de l'API depuis le fichier `gatsby-node.js`.

Voici comment faire. A la racine de votre site, créez un fichier nommé `gatsby-node.js`. 
Ensuite, ajoutez dans ce fichier ce qui suit:

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

La fonction `onCreateNode` sera appelée par Gatsby à chaque fois qu'un nouveau nœud est créé (ou mis à jour).

Arrêtez, puis redémarrez le serveur de développement. En faisant ceci, 
vous allez voir s'afficher dans la console un certain nombre de nouveaux nœuds fraîchement créés.

Dans la section suivante, vous allez utiliser l'API `onCreateNode` pour ajouter les slugs de vos pages markdown aux nœuds `MarkdownRemark`.

Modifiez votre fonction en sorte qu'elle signale uniquement les nœuds `MarkdownRemark`.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

Vous souhaitez créer les slugs des pages à partir des noms des fichiers markdown: 
de telle sorte que `pandas-and-bananas.md` deviendra `/pandas-and-bananas/`.
Mais comment obtenir le nom du fichier à partir du nœud `MarkdownRemark`? Vous devez ici
_parcourir_ le "graphe du nœud" jusqu'à atteindre son nœud _parent_ `File`, car les 
nœuds `File` contiennent l'information dont vous avez besoin à propos des fichiers présents sur disque.
Pour cela, modifiez de nouveau votre fonction:

```javascript:title=gatsby-node.js
// highlight-next-line
exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const fileNode = getNode(node.parent)
    console.log(`\n`, fileNode.relativePath)
    // highlight-end
  }
}
```

Après avoir redémarré votre serveur de développement, vous devriez voir les chemins
 relatifs de vos deux fichiers markdown affichés dans la console.

![markdown-relative-path](markdown-relative-path.png)

Maintenant vous devez créer les slugs. Comme cela peut rapidement devenir complexe, le plugin `gatsby-source-filesystem` fournit une fonction pour créer des slugs à partir des noms de fichiers.
Utilisons-la.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

Cette fonction prend en charge la recherche du nœud parent `File`, ainsi que la création du slug.
Relancez le serveur de développement et vous devriez voir dans la console deux slugs, 
un par fichier markdown.

Nous allons directement ajouter ces nouveaux slugs aux nœuds `MarkdownRemark`. C'est très astucieux, 
car toutes les données que vous ajoutez aux nœuds seront accessibles plus tard via des 
requêtes GraphQL. Ainsi, il sera facile de récupérer le slug quand il faudra créer les pages.

Pour faire cela, vous devez passer à votre implementation de l'API une fonction appelée
[`createNodeField`](/docs/actions/#createNodeField). Cette fonction vous permet d'ajouter 
des champs supplémentaires aux nœuds créés par d'autres plugins. En effet, seul le créateur originel
d'un nœud est autorisé à modifier directement ce nœud. Tous les autres plugins (votre `gatsby-node.js` 
y compris) doivent utiliser la fonction `createNodeField` pour créer des champs
additionnels.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)
// highlight-next-line
exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions // highlight-line
  if (node.internal.type === `MarkdownRemark`) {
    // highlight-start
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
    // highlight-end
  }
}
```

Redémarrez le serveur de développement et lancez ou actualisez GraphiQL. Puis
exécutez la requête GraphQL suivante pour visualiser vos nouveaux slugs.  

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        fields {
          slug
        }
      }
    }
  }
}
```

Maintenant que les slugs ont été créés, nous pouvons passer à la création des pages.

## Créer des pages

Dans le même fichier `gatsby-node.js`, ajoutez ce qui suit.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

// highlight-start
exports.createPages = async ({ graphql, actions }) => {
  // **Remarque:** La fonction graphql renvoie une Promise
  // voir: https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Global_Objects/Promise pour plus d'informations
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  console.log(JSON.stringify(result, null, 4))
}
// highlight-end
```

Vous avez ajouté une implementation de [`createPages`](/docs/node-apis/#createPages), API que Gatsby appelle pour ajouter des pages. 

Comme nous l'avons déjà dit dans l'introduction à ce tutoriel, pour créer des pages 
de façon dynamique, il faut:

1.  Récupérer les données avec GraphQL
2.  Affectez les résultats des requêtes aux pages 

Le code précédent est la première étape pour créer des pages à partir de vos fichiers
markdown: vous utilisez la fonction `graphql` pour récupérer les slugs markdown 
que vous avez créés. Ensuite vous affichez le résultat de cette requête, ce qui devrait 
ressembler à:

![query-markdown-slugs](query-markdown-slugs.png)

Pour créer des pages, vous avez besoin de quelque chose en plus des slugs:
un composant Template de page. Comme à peu près tout dans Gatsby, les pages dynamiques 
reposent sur des composants React. Lorsque vous créez une page, vous avez besoin 
d'indiquer quel composant sera utilisé pour la rendre.

Créez un répertoire `src/templates`, puis ajoutez ce qui suit dans un fichier nommé
`src/templates/blog-post.js`.

```jsx:title=src/templates/blog-post.js
import React from "react"
import Layout from "../components/layout"

export default () => {
  return (
    <Layout>
      <div>Hello blog post</div>
    </Layout>
  )
}
```

Ensuite mettez à jour le fichier `gatsby-node.js`

```javascript:title=gatsby-node.js
const path = require(`path`) // highlight-line
const { createFilePath } = require(`gatsby-source-filesystem`)

exports.onCreateNode = ({ node, getNode, actions }) => {
  const { createNodeField } = actions
  if (node.internal.type === `MarkdownRemark`) {
    const slug = createFilePath({ node, getNode, basePath: `pages` })
    createNodeField({
      node,
      name: `slug`,
      value: slug,
    })
  }
}

exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions // highlight-line
  const result = await graphql(`
    query {
      allMarkdownRemark {
        edges {
          node {
            fields {
              slug
            }
          }
        }
      }
    }
  `)

  // highlight-start
  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/templates/blog-post.js`),
      context: {
        // Les données passées via context sont accessibles
        // dans les requêtes de page en tant que variables GraphQL.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

Redémarrez votre serveur de développement et vos pages seront créées! Au stade de développement, une méthode facile pour retrouver les pages que vous venez de créer est de naviguer vers une adresse quelconque où Gatsby va gracieusement afficher la liste de toutes les pages du site.
Par exemple, si vous naviguez vers `http://localhost:8000/sdf`, vous allez voir les nouvelles pages que vous venez de créer.

![new-pages](new-pages.png)

Consultez une de ces pages et vous allez voir:

![hello-world-blog-post](hello-world-blog-post.png)

C'est un peu plat et ce n'est pas ce que vous voulez. Mais maintenant vous pouvez alimenter 
votre page avec des données de votre post markdown. Modifiez `src/templates/blog-post.js`:

```jsx:title=src/templates/blog-post.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-start
export default ({ data }) => {
  const post = data.markdownRemark
  // highlight-end
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

// highlight-start
export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
    }
  }
`
// highlight-end
```

Et…

![blog-post](blog-post.png)

Splendide!

La dernière étape est d'ajouter sur la page d'accueil des liens vers vos nouvelles pages.

Revenez dans `src/pages/index.js`, faites une recherche pour vos slugs markdown et créez les liens.

```jsx:title=src/pages/index.js
import React from "react"
import { css } from "@emotion/core"
import { Link, graphql } from "gatsby" // highlight-line
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  return (
    <Layout>
      <div>
        <h1
          css={css`
            display: inline-block;
            border-bottom: 1px solid;
          `}
        >
          Des Pandas Géniaux Qui Mangent Des Trucs 
        </h1>
        <h4>{data.allMarkdownRemark.totalCount} Posts</h4>
        {data.allMarkdownRemark.edges.map(({ node }) => (
          <div key={node.id}>
            {/* highlight-start */}
            <Link
              to={node.fields.slug}
              css={css`
                text-decoration: none;
                color: inherit;
              `}
            >
              {/* highlight-end */}
              <h3
                css={css`
                  margin-bottom: ${rhythm(1 / 4)};
                `}
              >
                {node.frontmatter.title}{" "}
                <span
                  css={css`
                    color: #555;
                  `}
                >
                  — {node.frontmatter.date}
                </span>
              </h3>
              <p>{node.excerpt}</p>
            </Link> {/* highlight-line */}
          </div>
        ))}
      </div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC }) {
      totalCount
      edges {
        node {
          id
          frontmatter {
            title
            date(formatString: "DD MMMM, YYYY")
          }
          // highlight-start
          fields {
            slug
          }
          // highlight-end
          excerpt
        }
      }
    }
  }
`
```

Et voilà! Un blog tout petit, mais opérationnel! 

## Challenge

Essayez de jouer avec le site. Par exemple, essayez d'y ajouter plus de fichiers markdown.
Essayez de récuperer d'autres données des nœuds `MarkdownRemark` et ajoutez ces données à la page 
d'accueil ou aux pages contenant les articles de blog.

Dans ce tutoriel, vous avez appris les bases de la construction de pages en utilisant la couche de données Gatsby. Vous avez appris à vous connecter à une _source_ de données et à _transformer_ les données en utilisant des plugins. Vous avez appris à utiliser GraphQL pour _mapper_ les données sur des pages. 
Enfin, vous avez appris à créer des _templates de page_ dans lesquels des requêtes récupérent les données à afficher sur chaque page.

## Quelle est la suite ?

Que faire maintenant que vous avez créé un magnifique site Gatsby?

- Partagez votre site Gatsby sur Twitter et découvrez ce que les autres ont développé en effectuant une recherche sur le hashtag #gatsbytutorial! 
Assurez-vous de mentionner @gatsbyjs dans votre Tweet et d'y inclure le hashtag #gatsbytutorial :)
- Jetez un coup d'œil à quelques [sites exemples](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Explorez l'univers des [plugins](/docs/plugins/)
- Découvrez ce que d'autres ont [créé à l'aide de Gatsby](/showcase/)
- Consultez la documentation concernant les [APIs de Gatsby](/docs/api-specification/), les [nœuds](/docs/node-interface/), ou [GraphQL](/docs/graphql-reference/)
