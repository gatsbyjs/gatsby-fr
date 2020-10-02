---
Titre: Créer des pages par programme à partir de données
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ce didacticiel fait partie d'une série sur la couche de données de Gatsby. Assurez-vous que vous avez traversé [partie 4](/tutorial/part-four/), [partie 5](/tutorial/part-five/), et [partie 6](/tutorial/part-six/) avant de continuer ici.

## Contenu de ce didacticiel?

Dans le didacticiel précédent, vous avez créé une jolie page d'index qui interroge le markdown
fichiers et produit une liste de titres et d'extraits d'articles de blog. Mais vous ne voulez pas seulement voir des extraits, vous voulez des pages réelles pour votre
fichiers de démarque.

Vous pouvez continuer à créer des pages en plaçant les composants React dans `src/pages`.Cependant, vous
maintenant apprends à _programmatically_ créer des pages à partir de _data_. Gatsby est _not_
limité à la création de pages à partir de fichiers comme de nombreux générateurs de sites statiques. Gatsby permet
vous utilisez GraphQL pour interroger votre _data_ et _map_ la requête aboutit à _pages_—tout à la construction
temps. C'est une idée vraiment puissante. Vous explorerez ses implications et
comment l'utiliser pour le reste de cette partie du didacticiel.

Commençons.
## Création slugs pour les pages

Un ‘slug’ est la partie d'identification unique d'une adresse Web,
comme le `/tutorial/part-seven` une partie de la page `https://www.gatsbyjs.org/tutorial/part-seven/`.

Il est également appelé «chemin», mais ce didacticiel utilisera le terme «slug» par souci de cohérence.

La création de nouvelles pages comporte deux étapes:

1. Générez le "path" ou "slug" pour la page.
2. Créez la page.

_**Remarque**: Souvent, les sources de données fournissent directement un slug ou un chemin d'accès pour le contenu - lorsque vous travaillez avec l'un de ces systèmes (e.g. a CMS), vous n'avez pas besoin de créer les slugs vous-même comme vous le faites avec les fichiers de démarque._

Pour créer vos pages de démarques, vous apprendrez à utiliser deux API Gatsby:
[`onCreateNode`](/docs/node-apis/#onCreateNode) et
[`createPages`](/docs/node-apis/#createPages). Ce sont deux APIs performantes
vous verrez utilisé dans de nombreux sites et plugins.

Nous faisons de notre mieux pour rendre les API Gatsby simples à mettre en œuvre. Pour implémenter une API, vous exportez une fonction
avec le nom de l'API de `gatsby-node.js`.

Alors, voici où vous ferez cela. À la racine de votre site, créez un fichier nommé
`gatsby-node.js`. Ajoutez ensuite ce qui suit.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

Ce `onCreateNode` La fonction sera appelée par Gatsby chaque fois qu'un nouveau nœud est créé (ou mis à jour).

Arrêtez et redémarrez le serveur de développement. Comme vous le faites, vous en verrez plusieurs
les nœuds créés sont enregistrés dans la console du terminal.

Dans la section suivante, vous utiliserez cette API pour ajouter des slugs pour vos pages Markdown à `MarkdownRemark`
nœuds.

Changez votre fonction pour qu'elle ne consigne plus que `MarkdownRemark` nœuds.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

Vous souhaitez utiliser chaque nom de fichier de démarque pour créer le slug de page. Alors
`pandas-and-bananas.md` va devenir `/pandas-and-bananas/`.Mais comment obtenez-vous
le nom de fichier du `MarkdownRemark` nœud? Pour l'obtenir, vous devez _traverse_
le "graphe de nœuds" à son _parent_ `File` nœud, comme `File`les nœuds contiennent des données que vous
besoin de fichiers sur le disque. Pour ce faire, modifiez à nouveau votre fonction:

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

Après avoir redémarré votre serveur de développement, vous devriez voir les chemins relatifs pour vos deux démarques
les fichiers s'impriment sur l'écran du terminal.

![markdown-relative-path](markdown-relative-path.png)

Vous devrez maintenant créer des slugs. Comme la logique de création de slugs à partir des noms de fichiers peut
délicat, le `gatsby-source-filesystem` plugin est livré avec une fonction pour créer
limaces. Utilisons ça.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

La fonction gère la recherche du parent `File` nœud avec la création du
limace. Exécutez à nouveau le serveur de développement et vous devriez voir connecté au terminal
deux slugs, un pour chaque fichier de démarque.

Vous pouvez maintenant ajouter vos nouveaux slugs directement sur le `MarkdownRemark` nœuds. C'est
puissant, car toutes les données que vous ajoutez aux nœuds peuvent être interrogées ultérieurement avec GraphQL.
Ainsi, il sera facile d'obtenir le slug au moment de créer les pages.

Pour ce faire, vous utiliserez une fonction transmise à votre implémentation d'API appelée
[`createNodeField`](/docs/actions/#createNodeField). Cette fonction
vous permet de créer des champs supplémentaires sur les nœuds créés par d'autres plugins. Seulement
le créateur d'origine d'un nœud peut directement modifier le nœud - tous les autres plugins
(including your `gatsby-node.js`) doit utiliser cette fonction pour créer des
des champs.
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

Redémarrez le serveur de développement et ouvrez ou actualisez GraphiQL. Alors lancez ceci
Requête GraphQL pour voir vos nouveaux slugs.
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

Maintenant que les slugs sont créés, vous pouvez créer les pages.

## Créer des pages

Dans le même `gatsby-node.js` fichier, ajoutez ce qui suit.

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
  // **Note:** L'appel de la fonction graphql renvoie une promesse
  // see: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise for more info
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

Vous avez ajouté une implémentation du
[`createPages`](/docs/node-apis/#createPages) API que Gatsby appelle pour que les plugins puissent ajouter
pages.

Comme mentionné dans l'introduction de cette partie du didacticiel, les étapes de création de pages par programme sont les suivantes:

1. Interroger des données avec GraphQL
2. Mappez les résultats de la requête aux pages

Le code ci-dessus est la première étape pour créer des pages à partir de votre démarque lorsque vous
en utilisant la fonction `graphql` fournie pour interroger les slugs de démarque que vous avez créés.
Ensuite, vous déconnectez le résultat de la requête qui devrait ressembler à:

![query-markdown-slugs](query-markdown-slugs.png)

Vous avez besoin d'une chose supplémentaire au-delà d'un slug pour créer des pages: un modèle de page
composant. Comme tout dans Gatsby, les pages programmatiques sont alimentées par React
Composants. Lors de la création d'une page, vous devez spécifier le composant à utiliser.

Créez un répertoire à `src/templates`, puis ajoutez ce qui suit dans un fichier nommé
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

Puis mettre à jour `gatsby-node.js`

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
        // Data passed to context is available
        // in page queries as GraphQL variables.
        slug: node.fields.slug,
      },
    })
  })
  // highlight-end
}
```

Redémarrez le serveur de développement et vos pages seront créées! Un moyen simple de
trouver de nouvelles pages que vous créez pendant le développement consiste à accéder à un chemin aléatoire où
Gatsby vous montrera utilement une liste de pages sur le site. Si vous allez à
`http://localhost:8000/sdf`, vous verrez les nouvelles pages que vous avez créées.
![new-pages](new-pages.png)

Visitez l'un d'entre eux et vous voyez:
![hello-world-blog-post](hello-world-blog-post.png)

Ce qui est un peu ennuyeux et pas ce que vous voulez. Vous pouvez maintenant extraire les données de votre publication de démarque. Changement
`src/templates/blog-post.js` à:

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

And…

![blog-post](blog-post.png)

Sucré!

La dernière étape consiste à créer un lien vers vos nouvelles pages à partir de la page d'index.

Retourner à `src/pages/index.js`, recherchez vos slugs de démarque et créez
liens.

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
          Amazing Pandas Eating Things
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

Et voilà! Un blog fonctionnel, quoique petit!

## Défi

Essayez de jouer davantage avec le site. Essayez d'ajouter d'autres fichiers de démarque. Explorer
interroger d'autres données du `MarkdownRemark` nœuds et en les ajoutant au
page d'accueil ou pages d'articles de blog.

Dans cette partie du didacticiel, vous avez appris les bases de la construction avec
La couche de données de Gatsby. Vous avez appris à _source_ et _transform_ données utilisant
plugins, comment utiliser GraphQL pour _map_ données aux pages, puis comment créer _page
template components_ où vous recherchez des données pour chaque page.
## Qu'est-ce qui va suivre?

Maintenant que vous avez créé un site Gatsby, où allez-vous ensuite?

- Partagez votre site Gatsby sur Twitter et voyez ce que d'autres personnes ont créé en recherchant #gatsbytutorial! Assurez-vous de mentionner @gatsbyjs dans votre Tweet et d'inclure le hashtag #gatsbytutorial :)
- Vous pouvez jeter un œil à certains [example sites](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Explore plus [plugins](/docs/plugins/)
- See what [d'autres personnes construisent avec Gatsby](/showcase/)
- Consultez la documentation sur [Gatsby's APIs](/docs/api-specification/), [nœuds](/docs/node-interface/), or [GraphQL](/docs/graphql-reference/)
