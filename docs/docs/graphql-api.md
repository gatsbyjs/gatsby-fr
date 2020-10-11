---
title: GraphQL API
tableOfContentsDepth: 2
---

Un grand avantage de Gatsby est une couche de données intégrée qui combine toutes les sources de données que vous configurez. Les données sont collectées au [moment de la construction](/docs/glossary#build) et automatiquement assemblées dans un [schéma](/docs/glossary#schema) qui définit comment les données peuvent être interrogées sur votre site.

Ce document sert de référence pour les fonctionnalités GraphQL intégrées à Gatsby, y compris les méthodes d'interrogation et de recherche de données, et la personnalisation de GraphQL pour les besoins de votre site.

## Premiers pas avec GraphQL

GraphQL est disponible dans Gatsby sans installation spéciale: un schéma est automatiquement déduit et créé lorsque vous exécutez `gatsby develop` ou` gatsby build`. Lorsque le site se compile, la couche de données peut être [explorée](/docs/running-queries-with-graphiql/) à: `http://localhost:8000/___graphql`

## Sourcing de données

Les données doivent être [sourcées](/docs/content-and-data/) -- ou ajoutées au schéma GraphQL -- pour être interrogées et extraites dans les pages à l'aide de GraphQL. Gatsby utilise [plugins source](/plugins/?=Gatsby-source) pour extraire les données.

**Remarque**: GraphQL n'est pas nécessaire: vous pouvez toujours [utiliser Gatsby sans GraphQL](/docs/using-gatsby-without-graphql/).

Pour obtenir des données avec un plugin existant, vous devez installer tous les packages nécessaires. De plus, vous devez ajouter le plugin au tableau des plugins dans `gatsby-config` avec toutes les configurations optionnelles. Si vous souhaitez extraire des données du système de fichiers pour une utilisation avec GraphQL, telles que des fichiers Markdown, des images, etc., reportez-vous à la [documentation sur la source de données du système de fichiers](/docs/sourcing-from-the-filesystem/) and [recipes](/docs/recipes/sourcing-data).

Pour obtenir des instructions sur l'installation de plugins à partir de npm, consultez les instructions dans la documentation sur [utilisation d'un plugin](/docs/using-a-plugin-in-your-site/).

Vous pouvez également [créer des plugins personnalisés](/docs/creating-plugins/) pour s'adapter à vos propres cas d'utilisation et extraire les données comme vous le souhaitez.

## Composants de requête et hooks

Les données peuvent être interrogées à l'intérieur des pages, des composants ou du fichier `gatsby-node.js`, en utilisant l'une de ces options:

- Le composant `pageQuery`
- Le component `StaticQuery`
- Le hook `useStaticQuery`

**Remarque**: En raison de la façon dont Gatsby traite les requêtes GraphQL, vous ne pouvez pas mélanger les requêtes de page et les requêtes statiques dans le même fichier. Vous ne pouvez pas non plus avoir plusieurs requêtes de page ou requêtes statiques dans un fichier.

Pour plus d’informations sur les composants de page et autres que de page en ce qui concerne les requêtes, consultez le guide de documentation sur [création avec des composants](/docs/building-with-components/#how-does-gatsby-use-react-components)

### `pageQuery`

`pageQuery` est un composant intégré qui récupère les informations de la couche de données dans les pages Gatsby. Vous pouvez avoir une requête de page par page. Il peut prendre des arguments GraphQL pour les variables de vos requêtes.

Une [page est créée dans Gatsby](/docs/page-creation/) par n'importe quel composant React dans le dossier `src / pages`, ou en appelant l'action` createPage` et en utilisant un composant dans les options `createPage` -- ce qui signifie qu'une` pageQuery` ne fonctionnera pas dans n'importe quel composant, uniquement dans les composants qui répondent à ces critères.

Reportez-vous également au [guide sur l'interrogation de données dans les pages avec requête de page](/docs/page-query/).

#### Params

Une requête de page n'est pas une méthode, mais plutôt une variable exportée à laquelle une chaîne `graphql` et un bloc de requête valide sont attribués comme valeur:

```javascript
export const pageQuery = graphql`
  query HomePageQuery {
    site {
      siteMetadata {
        description
      }
    }
  }
`
```

**Remarque**: la requête exportée dans un `const` n'a pas besoin d'être nommée` pageQuery`. Plus important encore, Gatsby recherche une chaîne `graphql` exportée à partir du fichier.

#### Retour

Lorsqu'elle est incluse dans un fichier de composant de page, une requête de page renvoie un objet de données qui est passé automatiquement au composant en tant que accessoire.

```jsx
// highlight-start
const HomePage = ({ data }) => {
  // highlight-end
  return (
    <div>
      Bonjour!
      {data.site.siteMetadata.description} // highlight-line
    </div>
  )
}
```

### `StaticQuery`

StaticQuery est un composant intégré permettant de récupérer des données de la couche de données de Gatsby dans des composants autres que des pages, tels qu'un en-tête, une navigation ou tout autre composant enfant.

Vous ne pouvez avoir qu'une seule `StaticQuery` par page: pour inclure les données dont vous avez besoin à partir de plusieurs sources, vous pouvez utiliser une requête avec plusieurs [champs racine](/docs/graphql-concepts/#query-fields). Il ne peut pas prendre de variables comme arguments.

Reportez-vous également au [guide sur l'interrogation de données dans les composants avec requête statique](/docs/static-query/).

#### Params

Le composant `StaticQuery` prend deux valeurs comme accessoires dans JSX:

- `query`: une chaîne de requête `graphql`
- `render`: un composant avec accès aux données renvoyées

```jsx
<StaticQuery
  query={graphql` //highlight-line
    query HeadingQuery {
      site {
        siteMetadata {
          title
        }
      }
    }
  `}
  render={(
    data //highlight-line
  ) => (
    <header>
      <h1>{data.site.siteMetadata.title}</h1>
    </header>
  )}
/>
```

#### Retour

Le composant StaticQuery renvoie `data` dans un prop `render`:

```jsx
<StaticQuery
  // ...
  // highlight-start
  render={data => (
    <header>
      <h1>{data.site.siteMetadata.title}</h1>
    </header>
  )}
  // highlight-end
/>
```

### `useStaticQuery`

Le hook `useStaticQuery` peut être utilisé comme` StaticQuery` dans n'importe quel composant ou page, mais ne nécessite pas l'utilisation d'un composant et d'un accessoire de rendu.

Comme il s'agit d'un hook React, les [règles des hooks](https://reactjs.org/docs/hooks-rules.html) s'appliquent et vous devrez l'utiliser avec React et ReactDOM version 16.8.0 ou ultérieure. En raison du fonctionnement actuel des requêtes dans Gatsby, une seule instance de `useStaticQuery` est prise en charge dans chaque fichier.

Reportez-vous également au [guide sur l'interrogation de données dans les composants avec useStaticQuery](/docs/use-static-query/).

#### Params

Le hook `useStaticQuery` prend un argument:

- `query`: une chaîne de requête `graphql`

```javascript
const data = useStaticQuery(graphql`
  query HeaderQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`)
```

#### Returns

Le hook `useStaticQuery` renvoie des données dans un objet:

```jsx
const data = useStaticQuery(graphql`
  query HeaderQuery {
    site {
      siteMetadata {
        title
      }
    }
  }
`)
return (
  // highlight-start
  <header>
    <h1>{data.site.siteMetadata.title}</h1>
  </header>
  // highlight-end
)
```

## Structure de la requête

Les requêtes sont écrites dans la même forme que celle dans laquelle vous souhaitez renvoyer les données. La manière dont vous sourcez les données déterminera les noms des champs sur lesquels vous pouvez effectuer des requêtes, en fonction des nœuds qu'elles ajoutent au schéma GraphQL.

Pour comprendre les parties d'une requête, reportez-vous au [guide conceptuel](/docs/graphql-concepts/#understanding-the-parts-of-a-query).

### Arguments de requête GraphQL

Les requêtes GraphQL peuvent prendre des arguments pour modifier la manière dont les données sont renvoyées. La logique de ces arguments est gérée en interne par Gatsby. Les arguments peuvent être passés dans des champs à n'importe quel niveau de la requête.

Différents nœuds peuvent accepter différents arguments en fonction de la nature du nœud.

Les arguments que vous pouvez passer aux collections (comme les tableaux ou les longues listes de données - par exemple `allFile` ou` allMdx`) sont:

- [`filter`](/docs/graphql-reference#filter)
- [`limit`](/docs/graphql-reference#limit)
- [`sort`](/docs/graphql-reference#sort)
- [`skip`](/docs/graphql-reference#skip)

Les arguments que vous pouvez passer à un champ `date` sont:

- [`formatString`](/docs/graphql-reference#dates)
- [`locale`](/docs/graphql-reference#dates)

Les arguments que vous pouvez passer à un champ `excerpt` sont:

- [`pruneLength`](/docs/graphql-reference#excerpt)
- [`truncate`](/docs/graphql-reference#excerpt)

### Opérations de requête Graphql

D'autres configurations intégrées peuvent être utilisées dans les requêtes

- [`Alias`](/docs/graphql-reference#alias)
- [`Group`](/docs/graphql-reference#group)

Pour des exemples, reportez-vous aux [recettes de requête](/docs/recipes/querying-data) et au [guide de référence des options de requête GraphQL](/docs/graphql-reference/).

## Requête fragments

Les fragments vous permettent de réutiliser des parties de requêtes GraphQL. Ils vous permettent également de diviser les requêtes complexes en composants plus petits et plus faciles à comprendre.

FPour plus d'informations, consultez le guide de la documentation sur [l'utilisation de fragments dans Gatsby](/docs/using-graphql-fragments/).

### Fragments de Gatsby

Certains fragments sont inclus dans les plugins Gatsby, tels que des fragments pour renvoyer des données d'image optimisées dans divers formats avec `gatsby-image` et` gatsby-transformer-sharp`, ou des fragments de données avec `gatsby-source-contentful`. Pour plus d'informations sur les plugins contenant des fragments, consultez le [`gatsby-image` README](/packages/gatsby-image#fragments).

## Personnalisations avancées

Vous pouvez personnaliser les données d'origine dans la couche GraphQL et créer des relations entre les nœuds avec les [API Gatsby Node](/docs/node-apis/).

Le schéma GraphQL peut être personnalisé pour des cas d'utilisation plus avancés: en savoir plus à ce sujet dans la [documentation de l'API de personnalisation du schéma](/docs/schema-customization/).
