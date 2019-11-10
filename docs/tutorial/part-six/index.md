---
title: Plugins transformateur
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ce tutoriel fait partie d’une série sur la couche de données de Gatsby. Assurez-vous d'avoir traversé la [partie 4](/tutorial/part-four/) et la [partie 5](/tutorial/part-five/) avant de continuer ici.

## Qu'est-ce qu'il y a dans ce tutoriel?

Le tutoriel précédent montrait comment les plugins source apportent les données _dans_ le système de données de Gatsby. Dans ce tutoriel, vous apprendrez comment les plugins transformateur _transforment_ le contenu brut apporté par les plugins sources. La combinaison de plugins source et de plugins transformateur peut prendre en charge toutes les sources de données et la transformation de données dont vous pourriez avoir besoin lors de la construction d'un site Gatsby.

## Plugins transformateur

Souvent, le format des données que vous obtenez des plugins source n'est pas ce que vous voulez.
pour construire votre site Web. Le plugin source du système de fichiers vous permet d'interroger les données
_à propos_ des fichiers mais que faire si vous voulez interroger des données _dans_ des fichiers?

Pour rendre cela possible, Gatsby supporte les plugins transformateur qui prennent le contenu brut des plugins 
source et les _transforment_ en quelque chose de plus utilisable.

Par exemple, les fichiers markdown. Markdown est agréable à écrire, mais lorsque vous construisez une page avec lui, vous avez besoin du markdown pour être HTML.

Ajoutez un fichier markdown à votre site à l'adresse suivante
`src/pages/sweet-pandas-eating-sweets.md` (Ceci deviendra votre premier markdown
blog post) et apprenez comment _transformer_ cela en HTML à l'aide de plugins transformateur et de
GraphQL.

```markdown:title=src/pages/sweet-pandas-eating-sweets.md
---
title: "Sweet Pandas Eating Sweets"
date: "2017-08-10"
---

Pandas are really sweet.

Here's a video of a panda eating sweets.

<iframe width="560" height="315" src="https://www.youtube.com/embed/4n0xNbfJLR8" frameborder="0" allowfullscreen></iframe>
```

Une fois que vous avez sauvegardé le fichier, regardez encore une fois `/my-files/`—le nouveau fichier markdown est dans
la table. C'est une caractéristique très puissante de Gatsby. Comme le premier exemple de
`siteMetadata`, les plugins source peuvent recharger les données en direct.
`gatsby-source-filesystem` est toujours à la recherche de nouveaux fichiers à ajouter et quand ils le sont, exécute à nouveau vos requêtes.

Ajouter un plugin transformateur qui peut transformer les fichiers markdown:

```shell
npm install --save gatsby-transformer-remark
```

Ensuite, ajoutez-le au `gatsby-config.js` comme d'habitude:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    `gatsby-transformer-remark`, // highlight-line
    `gatsby-plugin-emotion`,
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

Redémarrez le serveur de développement puis rafraîchissez (ou ou ouvrez à nouveau) GraphiQL et regardez 
à l'auto-complétion:

![markdown-autocomplete](markdown-autocomplete.png)

Sélectionnez à nouveau `allMarkdownRemark` et exécutez-le comme vous l'avez fait pour `allFile`. Vous allez
y voir le fichier de démarque que vous avez récemment ajouté. Explorez les domaines qui sont
disponible sur le nœud `MarkdownRemark`.

![markdown-query](markdown-query.png)

Ok! Avec un peu de chance, les bases commencent à se mettre en place. Les plugins source amènent les données
_dans_ le système de données de Gatsby et les plugins _transformateur_ transforment le contenu brut apporté par les plugins source. Ce modèle peut gérer toutes les sources de données et la transformation de données dont vous pourriez avoir besoin lors de la construction d'un site Gatsby.

## Créez une liste des fichiers markdown de votre site dans `src/pages/index.js`

Vous devez maintenant créer une liste de vos fichiers markdown sur la page d'accueil. Comme beaucoup de blogs, vous 
voulez finir avec une liste de liens sur la page d'accueil pointant vers chaque article du blog. Avec GraphQL 
vous pouvez _interroger_ pour la liste courante des markdown des articles du blog afin de ne pas avoir besoin de 
maintenir la liste manuellement.

Comme pour la page `src/pages/my-files.js`, remplacez `src/pages/index.js` par
ce qui suit pour ajouter une requête GraphQL avec du HTML et du style initial.

```jsx:title=src/pages/index.js
import React from "react"
import { graphql } from "gatsby"
import { css } from "@emotion/core"
import { rhythm } from "../utils/typography"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
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
          </div>
        ))}
      </div>
    </Layout>
  )
}

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

Maintenant, la page d'accueil devrait ressembler:

![frontpage](frontpage.png)

Mais votre seul article du blog semble un peu isolé. Ajoutons-en un autre à l'adresse
`src/pages/pandas-and-bananas.md`

```markdown:title=src/pages/pandas-and-bananas.md
---
title: "Pandas and Bananas"
date: "2017-08-21"
---

Do Pandas eat bananas? Check out this short video that shows that yes! pandas do
seem to really enjoy bananas!

<iframe width="560" height="315" src="https://www.youtube.com/embed/4SZl1r2O_bY" frameborder="0" allowfullscreen></iframe>
```

![two-posts](two-posts.png)

Ce qui est super ! Sauf que... l'ordre des messages est incorrect.

Mais c'est facile à réparer. Lorsque vous interrogez une connexion d'un certain type, vous pouvez passer 
une variété d'arguments à la requête GraphQL. Vous pouvez `trier` et `filtrer` les nœuds, définir le nombre 
de nœuds à `sauter`, et choisir la `limite` du nombre de nœuds à récupérer. Grâce à ce puissant ensemble 
d'opérateurs, vous pouvez sélectionner toutes les données que vous voulez—dans le format dont vous avez besoin.

Dans la requête GraphQL de votre page d'index, changez `allMarkdownRemark` en
`allMarkdownRemark(sort: { fields: [frontmatter___date], order: DESC })`. _Note: Il y a 3 soulignements entre `frontmatter` 
et `date`._ Sauvegardez ceci et l'ordre de tri devrait être fixé.

Essayez d'ouvrir GraphiQL et de jouer avec différentes options de tri. Vous pouvez trier la connexion
`allFile` avec d'autres connexions.

Pour plus de documentation sur nos opérateurs de requêtes, consultez notre [guide de référence GraphQL.](/docs/graphql-reference/)

## Challenge

Essayez de créer une nouvelle page contenant un article du blog et voyez ce qui arrive à la liste des articles du blog sur la page d'accueil !

## Quelle est la prochaine étape?

C'est génial ! Vous venez de créer une belle page d'index où vous interrogez vos fichiers markdown et produisez une liste de titres d'articles du blog et d'extraits. Mais vous ne voulez pas seulement voir des extraits, vous voulez des pages réelles pour vos fichiers markdown.

Vous pouvez continuer à créer des pages en plaçant les composants React dans `src/pages`. Cependant, vous apprendrez ensuite comment créer des pages par _programmation_ à partir des _données_. Gatsby n'est _pas_
limité à faire des pages à partir de fichiers comme beaucoup de générateurs de sites statiques. Gatsby vous permet d'utiliser GraphQL pour interroger vos _données_ et _schématise_ les résultats de la requête vers les _pages_—tout au build.
C'est une idée très puissante. Vous explorerez ses implications et les façons de l'utiliser dans le prochain tutoriel, où vous apprendrez à [créer des pages par programmation à partir de données](/tutorial/part-seven/).
