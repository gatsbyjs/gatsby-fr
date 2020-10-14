---
title: "Recettes: Récupération de données"
tableOfContentsDepth: 1
---

Gatsby vous permet d'accéder aux données de toutes vos sources en utilisant une seule interface GraphQL.

## Récupérer les données via une requête de page (Page Query)

Vous pouvez utiliser le tag `graphql` pour récupérer des données dans les pages de votre site Gatsby. Cela vous permet d'avoir accès à tout ce qui est inclus dans la couche de données Gatsby (par exemple, les métadonnées de votre site, les plugins source, les images et bien plus encore).  

### Instructions

1. Importez `graphql` de `gatsby`.

2. Exportez une constante nommée `query` et définissez-la égale au template littéral `graphql` avec la requête comprise entre deux guillemets obliques (`).

3. Passez `data` comme prop au composant.

4. La variable `data` contient les données récupérées. Vous pouvez y faire référence dans votre code JSX pour produire du HTML.

```jsx:title=src/pages/index.js
import React from "react"
// highlight-next-line
import { graphql } from "gatsby"

import Layout from "../components/layout"

// highlight-start
export const query = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end

// highlight-next-line
const IndexPage = ({ data }) => (
  <Layout>
    // highlight-next-line
    <h1>{data.site.siteMetadata.title}</h1>
  </Layout>
)

export default IndexPage
```

### Ressources additionnelles

- [GraphQL et Gatsby](/docs/graphql/): comprendre la forme attendue de vos données
- [Plus d'informations sur comment récupérer les données dans les pages avec GraphQL](/docs/page-query/)
- [MDN concernant les littéraux de gabarits étiquetés](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Litt%C3%A9raux_gabarits) semblables à ceux utilisés par GraphQL

## Récupérer les données via un composant StaticQuery

`StaticQuery` est un composant qui permet à des composants [NonPageComponent](/docs/static-query/) (tels qu'un en-tête, un composant de navigation ou tout autre composant enfant) de récupérer les informations de la couche de données Gatsby.

### Instructions

1. Le composant `StaticQuery` nécessite deux props de rendu: `query` et `render`.

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { StaticQuery, graphql } from "gatsby"

const NonPageComponent = () => (
  <StaticQuery
    query={graphql` // highlight-line
      query NonPageQuery {
        site {
          siteMetadata {
            title
          }
        }
      }
    `}
    render={(
      data // highlight-line
    ) => (
      <h1>
        Titre récupéré par le NonPageComponent via une StaticQuery:
        {data.site.siteMetadata.title}
      </h1>
    )}
  />
)

export default NonPageComponent
```

2. Vous pouvez alors utiliser ce composant comme n'importe quel autre [NonPageComponent](/docs/building-with-components#non-page-components) en l'important dans une page plus complexe, contenant d'autres composants JSX et des balises HTML.

## Récupérer les données via un hook useStaticQuery

Depuis la version v2.1.0 de Gatsby, vous pouvez utiliser le hook `useStaticQuery` pour récupérer les données en utilisant une fonction JavaScript plutôt qu'un composant. En effet, cette syntaxe permet de ne plus utiliser le composant `<StaticQuery>` pour enrober toutes les requêtes, ce que certains trouveront plus simple.

Le hook `useStaticQuery` prend en paramètre une requête GraphQL et retourne les résultats de la requête. Ce hook peut être assigné à une variable et être utilisé plus tard dans vos templates JSX.

### Conditions préalables

- Vous aurez besoin des versions 16.8.0 (ou ultérieures) de React et ReactDOM (maintenir Gatsby à jour permet de ne pas s'en préoccuper).
- Lecture recommandées: les [règles des Hooks React](https://fr.reactjs.org/docs/hooks-rules.html)

### Instructions

1. Pour pouvoir interroger les données via un hook, importez `useStaticQuery` et `graphql` de `gatsby`.

2. Au début d'un composant fonctionnel sans état, assignez à une variable la valeur de `useStaticQuery` avec votre requête `graphql` passée en tant qu'argument.

3. Dans le code JSX renvoyé par votre composant, vous pouvez faire référence à cette variable pour manipuler les résultats de la requête.

```jsx:title=src/components/NonPageComponent.js
import React from "react"
import { useStaticQuery, graphql } from "gatsby" //highlight-line

const NonPageComponent = () => {
  // highlight-start
  const data = useStaticQuery(graphql`
    query NonPageQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `)
  // highlight-end
  return (
    <h1>
      Titre récupéré par le composant non-page: {data.site.siteMetadata.title}{" "}
      //highlight-line
    </h1>
  )
}

export default NonPageComponent
```

### Ressources additionnelles

- [Plus sur l'utilisation de StaticQuery dans les composants pour récupérer les données](/docs/static-query/)
- [Différence entre une StaticQuery et une requête de page](/docs/static-query/#how-staticquery-differs-from-page-query)
- [Plus d'informations sur le hook useStaticQuery](/docs/use-static-query/)
- [Visualiser vos données à l'aide de GraphiQL](/docs/introducing-graphiql/)

## Limiter les résultats d'une requête avec GraphQL

Lorsque vous interrogez les données en utilisant le language GraphQL, vous pouvez limiter le nombre de résultats renvoyés en spécifiant un maximum. Cela peut être pratique lorsque vous avez besoin seulement d'une petite partie des données ou si vous mettez en place une [pagination](/docs/adding-pagination/).

Pour limiter les données, vous aurez besoin d'un site Gatsby ayant quelques noeuds dans sa couche de données GraphQL. Tous les sites disposent de noeuds tel que `allSitePage` et `sitePage`, créés automatiquement. D'autres noeuds peuvent être ajoutés en installant des plugins source tel que `gatsby-source-filesystem` dans le fichier `gatsby-config.js`.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start/)

### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement.
2. Accédez à l'explorateur GraphiQL dans un navigateur: `http://localhost:8000/___graphql`.
3. Pour commencer, dans l'éditeur, ajoutez une requête sur `allSitePage` avec les champs suivants:

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. Ajoutez un argument `limit` au champ `allSitePage` et attribuez-lui la valeur entière `3`.

```graphql
{
  allSitePage(limit: 3) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}
```

5. Sur la page GraphiQL, cliquez sur le bouton "Lecture". Le nombre de résultats dans le champ `edges` sera borné par le nombre spécifié.

### Ressources additionnelles

- En savoir plus sur les [noeuds dans notre API de données GraphQL](/docs/node-interface/)
- [Notre guide de référence GraphQL concernant la limitation de données retournées](/docs/graphql-reference/#limit)
- Un exemple en ligne:

<iframe
  title="Limiter les données obtenues"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## Trier les données avec GraphQL

L'ordre des résultats d'une requête GraphQL peut être spécifié via l'argument `sort`. Vous pouvez spécifier l'ordre de tri et les champs en fonction desquels le tri sera effectué.

Pour cette recette, vous aurez besoin d'un site Gatsby avec une collection de noeuds à trier dans sa couche de données GraphQL. Tous les sites disposent de noeuds tel que `allSitePage`, créé automatiquement: d'autres noeuds peuvent être ajoutés en installant des plugins source.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start)
- Des champs interrogeables préfixés par `all`, par exemple `allSitePage`

### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement.
2. Accédez à l'explorateur GraphiQL dans un navigateur: `http://localhost:8000/___graphql`
3. Pour commencer, dans l'éditeur, ajoutez une requête GraphQL sur `allSitePage` avec les champs suivants:

```graphql
{
  allSitePage {
    edges {
      node {
        id
        path
      }
    }
  }
}
```

4. Ajoutez un argument `sort` au champ `allSitePage` et assignez-lui un objet ayant comme attributs `fields` et `order`. La valeur de `fields` peut être un champ ou un tableau de champs, en fonction desquels le tri sera effectué (dans cet exemple, nous trions suivant le champ `path`). L'attribut `order` est égal soit à `ASC`, soit à `DESC`, pour un tri dans l'ordre croissant ou décroissant respectivement.

```graphql
{
  allSitePage(sort: {fields: path, order: ASC}) { // highlight-line
    edges {
      node {
        id
        path
      }
    }
  }
}

```

5. Sur la page GraphiQL, cliquez sur le bouton "Lecture": ainsi les données renvoyées seront triées suivant un ordre croissant des valeurs du champ `path`.

### Ressources additionnelles

- [Notre guide de référence GraphQL concernant le tri](/docs/graphql-reference/#sort)
- En savoir plus sur les [noeuds dans notre API de données GraphQL](/docs/node-interface/)
- Un exemple en ligne:

<iframe
  title="Trier les données"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## Filtrer les données avec GraphQL

Les résultats obtenus peuvent être filtrés en utilisant des opérateurs tels que `eq` (égal), `ne` (différent), `in` (dans) et `regex` (expression régulière) sur les champs à spécifier.

Pour cette recette, vous aurez besoin d'un site Gatsby avec une collection de noeuds à filtrer dans sa couche de données GraphQL. Tous les sites disposent de noeuds tel que `allSitePage`, créé automatiquement. D'autres noeuds peuvent être ajoutés en installant des plugins source et des plugins de transformation tels que `gatsby-source-filesystem` et `gatsby-transformer-remark`.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start)
- Des champs interrogeables préfixés par `all`, par exemple `allSitePage` ou `allMarkdownRemark`

### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement.
2. Accédez à l'explorateur GraphiQL dans un navigateur: `http://localhost:8000/___graphql`
3. Dans l'éditeur, ajoutez une requête sur un champ préfixé par 'all', par exemple sur `allMarkdownRemark` (ce qui signifie que la requête va renvoyer une liste de noeuds)

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

4. Ajoutez un argument `filter` au champ `allMarkdownRemark` et assignez-lui un objet ayant comme attributs les noms des champs par lesquels vous voulez filtrer. Dans cet exemple, le contenu Markdown est filtré suivant la valeur de l'attribut `categories` de la métadonnée frontmatter. La valeur suivante est l'opérateur: dans cet exemple, `eq`, l'opérateur `égalité`, ayant comme valeur 'creatures magiques'.

```graphql
{
  allMarkdownRemark(filter: {frontmatter: {categories: {eq: "creatures magiques"}}}) { // highlight-line
    edges {
      node {
        frontmatter {
          title
          categories
        }
      }
    }
  }
}
```

5. Sur la page GraphiQL, cliquez sur le bouton "Lecture". Uniquement les données qui vérifient les règles du filtre seront renvoyées: dans cet exemple, il s'agit uniquement de fichiers source Markdown étiquetés dans la catégorie 'creatures magiques'.

### Ressources additionnelles

- [Notre guide de référence GraphQL concernant le filtrage](/docs/graphql-reference/#filter)
- [Liste complète des opérateurs](/docs/graphql-reference/#complete-list-of-possible-operators)
- En savoir plus sur les [noeuds dans notre API de données GraphQL](/docs/node-interface/)
- Un exemple en ligne:

<iframe
  title="Filtrer les données"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## Alias de requête GraphQL

Dans une requête GraphQL, vous pouvez remplacer n'importe quel champ par un alias.

Si vous souhaitez exécuter deux requêtes sur la même source de données, vous pouvez utiliser un alias pour éviter un conflit dans la résolution de noms en ayant deux requêtes de même noms. 

### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement.
2. Accédez à l'explorateur GraphiQL dans un navigateur: `http://localhost:8000/___graphql`
3. Dans l'éditeur, ajoutez une requête en utilisant deux champs ayant le même nom, par exemple `allFile`

```graphql
{
  allFile {
    totalCount
  }
  allFile {
    pageInfo {
      currentPage
    }
  }
}
```

4. Ajoutez le nom que vous voulez utiliser comme alias avant le nom du champ de votre schéma GraphQL, séparé par deux-points. (Par exemple: `[alias-a-utiliser]: [nom-initial]`)

```graphql
{
  fileCount: allFile { // highlight-line
    totalCount
  }
  filePageInfo: allFile { // highlight-line
    pageInfo {
      currentPage
    }
  }
}
```

5. Sur la page GraphiQL, cliquez sur le bouton "Lecture". Les résultats de la requête sont deux objets ayant comme noms les alias que vous avez définis.

### Ressources additionnelles

- [Notre guide de référence GraphQL concernant les alias](/docs/graphql-reference/#aliasing)
- Un exemple en ligne:

<iframe
  title="Utiliser les alias"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

## Fragments de requête GraphQL

Les Fragments GraphQL sont des bouts de requêtes partageables et réutilisables.

Vous pouvez les utiliser pour avoir en commun plusieurs champs dans différentes requêtes ou pour stocker ("colocaliser") les composants à côté de données qu'ils utilisent.

### Instructions

1. Définissez un template `graphql` qui inclut un Fragment. Un Fragment est constitué par le mot-clé `fragment`, un nom arbitraire, le type GraphQL auquel sera associé le Fragment (dans cet exemple, le type `Site`, comme l'indique `on Site`), ainsi que par les champs qui forment ce Fragment:

```jsx
export const query = graphql`
  // highlight-start
  fragment SiteInformation on Site {
    title
    description
  }
  // highlight-end
`
```

2. Maintenant, utilisez ce Fragment dans une requête sur un champ de type associé au Fragment. Les résultats de la requête comporteront les champs du Fragment sans que vous ayez besoin de les déclarer explicitement:

```diff
export const pageQuery = graphql`
  query SiteQuery {
    site {
-     title
-     description
+   ...SiteInformation
    }
  }
`
```

**Remarque**: Les Fragments n'ont pas besoin d'être importés dans Gatsby. Exporter une requête comportant un Fragment rend ce Fragment disponible et réutilisable par _toutes_ les requêtes de votre projet.

Les Fragments peuvent être imbriqués dans d'autres Fragments et plusieurs Fragments peuvent être utilisés dans une même requête.

### Ressources additionnelles

- [Un simple répertoire exemple utilisant les Fragments](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Notre référence GraphQL concernant les Fragments](/docs/graphql-reference/#fragments)
- Les [Fragments Gatsby pour les images](/docs/gatsby-image/#image-query-fragments)
- [Un répertoire exemple pour la colocation des données](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## Interroger des données côté client avec `fetch`

On n'est pas obligé de récupérer les données uniquement pendant l'étape de construction et de se servir uniquement de ces données statiques. Vous pouvez interroger les données de manière dynamique, en cours d'exécution, de la même manière que vous récuperez les données dans une application React classique.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start/)
- Un composant de page, tel que `index.js`

### Instructions

1. Dans un fichier qui contient une définition de composant React, tel qu'une page dans `src/pages` ou un composant de mise en page, importez les hooks React `useState` et `useEffect`.

```jsx:title=src/pages/index.js
import React, { useState, useEffect } from "react"
```

2. A l'intérieur du composant, enrobez la fonction qui interroge les données dans le hook `useEffect`, de telle sorte que le composant va récupérer les données de manière asynchrone lorsque le composant sera monté dans le navigateur client. Ensuite, patientez (`await`) jusqu'à obtenir la réponse de l'API interrogée par `fetch` et appelez la fonction de mise à jour fournie par le hook `useState` (dans cet exemple, `setStarsCount`) pour sauvegarder dans la variable d'état (`starsCount`) les données obtenues via `fetch`.

```jsx:title=src/pages/index.js
import React, { useState, useEffect } from "react"

const IndexPage = () => {
  // highlight-start
  const [starsCount, setStarsCount] = useState(0)
  useEffect(() => {
    // envoyer une requête à l'API de GitHub
    fetch(`https://api.github.com/repos/gatsbyjs/gatsby`)
      .then(response => response.json()) // convertir la réponse en JSON
      .then(resultData => {
        setStarsCount(resultData.stargazers_count)
      }) // définir le nombre d'étoiles
  }, [])
  // highlight-end

  return (
    <section>
      // highlight-start
      <p>Données au moment de l'exécution: Nombre d'étoiles du répertoire Gatsby {starsCount}</p>
      // highlight-end
    </section>
  )
}

export default IndexPage
```

### Ressources additionnelles

- Guide consacré à la [récupération de données côté client](/docs/data-fetching/)
- Un [exemple de site](https://gatsby-data-fetching.netlify.com/) qui utilise cette recette
