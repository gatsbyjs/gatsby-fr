---
title: Créer des pages dynamiquement depuis des données
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ce tutoriel est inclu dans la série sur les niveau de données de Gatsby. Soyez sur d'avoir lu [part 4](/tutorial/part-four/), [part 5](/tutorial/part-five/), et [part 6](/tutorial/part-six/) avant de continuer ici.

## Qu'y a-t-il dans ce tutoriel?

Dans le tutoriel précédent vous avez créé une belle page d'accueil qui va chercher des fichiers
markdown and produit une liste d'article de blog avec titre et extraits. Mais vous ne voulez pas juste voir
les extraits mais les pages complètes pour les fichiers markdown.

Vous pouvez continuer à créer des pages en plaçant des composants React dans `src/pages`. Par contre vous
allez maintenant apprendre comment _programmatiquement_ créer des pages depuis _data_. Gatsby _n'est_ pas
limité à concevoir des pages depuis des fichiers comme beaucoup de générateurs de sites statiques. Gatsby vous permet d'utiliser GraphQL pour rechercher vos _données_ et _mapper_ le résultat en _pages_-tout cela à l'état de build. C'est réellement une idée puissante. Vous allez explorer ces implications et les façons de s'en servir sur la suite de ce tutoriel.

Commencons

## Créer des slugs pour les pages

Créer de nouvelles pages nécessite deux étapes:

1.  Générer le "chemin" ou "slug" pour la page.
2.  Créer la page.

_**Note**: Souvent les données sources vont directement fournir un slug ou chemin pour le contenu - quand vous travaillez avec un tel système (ex. un CMS), vous n'avez pas besoin de créer le slug vous même comme vous le faites avec les fichiers markdown._

Pour créer vos pages markdown vous allez apprendre à utiliser deux APIs de Gatsby:
[`onCreateNode`](/docs/node-apis/#onCreateNode)
[`createPages`](/docs/node-apis/#createPages).
Ces deux APIs sont utilisées dans de nombreux sites et plugins

Nous nous efforçons de faire les APIs de Gatsby simple à implementer. Pour implémenter une API vous devez exporter la fonction avec le nom de l'API depuis `gatsby-node.js`.

Alors voila ou vous allez le faire. A la racine de votre site, créez un fichier nommé `gatsby-node.js`. Puis ajoutez le code suivant.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  console.log(node.internal.type)
}
```

Cette fonction `onCreateNode` va être appelée par Gatsby a chaque fois qu'un nouveau noeud est créé (ou mis à jour).

Eteignez et relancez le serveur de développement. En le faisant, vous allez voir de nouveaux noeuds créés être affiché dans la console du terminale

Utilisez cette API pour ajouter le slug pour les pages markdown dans le nodes `MarkdownRemark`.

Changez la fonction pour qu'elle affiche  maintenant seulement les noeuds `MarkdownRemark`.

```javascript:title=gatsby-node.js
exports.onCreateNode = ({ node }) => {
  // highlight-start
  if (node.internal.type === `MarkdownRemark`) {
    console.log(node.internal.type)
  }
  // highlight-end
}
```

Vous voulez utiliser chaque nom de fichier markdown pour créer le slug de la page. Donc `pandas-et-bananes.md` va devenir `/pandas-et-bananes/`. Mais comment obtenir le nom du fichier depuis le noeud `MarkdownRemark`? POur y accéder vous devez _traverser_ le "graph de noeud" à son noeud _parent_ `File`, ce noeud contenant la donnée recherchée au sujet des fichiers sur le disque. Pour cela, modifiez votre fonction encore une fois:

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

Après avoir relancé votre serveur de développement vous devriez voir affiche sur l'écran du terminal les chemins relatifs pour vos deux fichiers markdown

![markdown-relative-path](markdown-relative-path.png)

Maintenant vous allez devoir créer des slugs. Comme la logique de concevoir des slugs depuis les fichiers peut être complexe, le plugin `gatsby-source-filesystem` inclus une fonction pour concevoir les slugs. Nous allons nous en servir.

```javascript:title=gatsby-node.js
const { createFilePath } = require(`gatsby-source-filesystem`) // highlight-line

exports.onCreateNode = ({ node, getNode }) => {
  if (node.internal.type === `MarkdownRemark`) {
    console.log(createFilePath({ node, getNode, basePath: `pages` })) // highlight-line
  }
}
```

La fonction s'occupe de trouver le noeud `File` parent et de créer en parallèle le slug. Relancer le serveur de développement et vous devriez voir afficher, dans le terminal, deux slugs, un par fichier markdown.

Maintenant vous pouvez ajouter votre nouveau slug directement dans les noeuds `MarkdownRemark`. C'est puissant, comme toutes les données ajoutées aux noeuds est disponible à la recherche via GraphQL.
Du coup, il sera facile de recuperer le slug quant il faudra créer les pages.

Pour ce faire, vous allez utiliser une fonction passée à votre implémentation de l'API qui s'appelle [`createNodeField`](/docs/actions/#createNodeField). Cette fonction vous permet de créer des champs additionnels dans les noeuds créés par d'autres plugins. Seul le créateur original d'un noeud peut le modifier directement - tous les autres plugins ()incluant `gatsby-node.js`) doivent utiliser cette fonction pour ajouter des champs.

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

Redémarrer le serveur de développement et ouvrez ou rafraîchissez GraphiQUE. Puis lancer cette requête GraphQL pour voir les nouveaux slugs.

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

Dans le même fichier `gatsby-node.js`, ajouter le code suivant.

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
  // **Note:** La fonction graphql retourne une Promise
  // voir pour plus d'infos: https://developer.mozilla.org/fr-FR/docs/Web/JavaScript/Reference/Global_Objects/Promise
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

Vous avez ajouté une implémentation de l'API [`createPages`](/docs/node-apis/#createPages) que Gatsby appelle afin d'ajouter des pages.

As mentioned in the intro to this part of the tutoriel, the steps to programmatically creating pages are:
Comment mentionné dans l'introduction de cette partie du tutoriel, pour créer dynamiquement des pages il faut:

1.  Recuperer des donnees avec GraphQL
2.  Affecter les resultats aux pages

Le code précédent est la première étape pour créer des pages depuis le markdown en utilisant la fonction `graphql` pour rechercher les slugs des markdowns créés.
Puis vous affichez le résultat de la requête qui devrait ressembler à:

![query-markdown-slugs](query-markdown-slugs.png)

Vous avez besoin de quelque chose de plus que le slug pour créer des pages: une page de template. Comme tout dans Gatsby, les pages dynamiques sont pourvues par les composants React. Lorsque vous créez une page, vous devez spécifier quel composant utiliser.

Créer un repertoire `src/templates`, puis ajouter le code suivant dans un fichier nommé `src/templates/blog-post.js`.

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

Puis mettre a jour `gatsby-node.js`

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

Redémarrer le serveur de développement et vos pages seront créées! Une méthode facile pour trouver les pages créées pendant le développement est d'aller sur une adresse aléatoire ou gatsby va généreusement afficher la liste des pages du site. Si vous allez sur <http://localhost:8000/sdf>, vous verrez les nouvelles pages.

![new-pages](new-pages.png)

Visiter en un parmis tous et vous verrez
![hello-world-blog-post](hello-world-blog-post.png)

Ce qui est un peu ennuyeux et pas exactement ce que vous voulez. Maintenant vous pouvez récupérer les données depuis votre post markdown. Changer `src/templates/blog-post.js` en:

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

Et...

![blog-post](blog-post.png)

Magnifique!

La dernière étape est d'ajouter un lien de l'accueil à vos nouvelles pages.

Retourner sur `src/pages/index.js`, rechercher pour votre slugs markdown et créer les liens.

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
          Incroyable Pandas mangeant des choses
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
                    color: #bbb;
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

Et voila ! Un blog, minimaliste, mais fonctionnel!

## Challenge

Essayez de jouer un peu plus avec le site. Essayez d'ajouter d'autres fichiers markdown. Explorer la recherche d'autres données depuis le noeud `MarkdownRemark` et de les ajouter à la page d'accueil ou la page des articles.

Dans cette partie du tutoriel, vous avez appris les fondations pour utiliser le data-layer de Gatsby. Vous avez appris comment _sourcer_ et _transformer_ les données en utilisant des plugins, comment utiliser GraphQL pour _mapper_ les données aux pages et enfin comment construire des _composants templates_ ou vous faites la recherche pour les donnees de chaque pages.

## La suite ?

Maintenant que vous avez développé un site avec Gatsby, que faire ?

- Partager votre site Gatsby sur twitter et voir ce que d'autres personnes ont créé en cherchant #gatsbytutorial! Soyez sûr de mentionner @gatsbyjs dans votre tweet et d’inclure le hashtag #gatsbytutorial! :)
- Vous pouvez jeter un oeil a certains [sites d'exemple](https://github.com/gatsbyjs/gatsby/tree/master/examples#gatsby-example-websites)
- Explorer plus de [plugins](/docs/plugins/)
- Voir ce que [d'autres personnes construisent avec Gatsby](/showcase/)
- Regarde la documentation sur les [APIs de Gatsby](/docs/api-specification/), [nodes](/docs/node-interface/), ou [GraphQL](/docs/graphql-reference/)
