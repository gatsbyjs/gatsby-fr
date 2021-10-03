---
title: "Recettes: Pages et Mise en page"
tableOfContentsDepth: 1
---

Ajoutez des pages à votre site Gatsby et utilisez les Mises en page _(NdT: layouts)_ pour gérer des élèments de mise en page communs à plusieurs pages.

## Structure de projet

A l'intérieur d'un projet Gatsby, vous pouvez avoir tous ou une partie des fichiers et des répertoires suivants:

```text
|-- /.cache
|-- /plugins
|-- /public
|-- /src
    |-- /pages
    |-- /templates
    |-- html.js
|-- /static
|-- gatsby-config.js
|-- gatsby-node.js
|-- gatsby-ssr.js
|-- gatsby-browser.js
```

Les fichiers les plus importants sont:

- `gatsby-config.js` — contient les options de votre site Gatsby, par exemple, des metadonnées _(NdT: metadata)_ comme le titre du project, sa description, les plugins utilisés, etc.
- `gatsby-node.js` — implemente les APIs Node.js Gatsby’s pour personnaliser les paramètres affectant le processus de build/compilation.
- `gatsby-browser.js` — permet de personnaliser les paramètres affectant le browser, en utilisant les APIs browser de Gatsby.
- `gatsby-ssr.js` — utilise les APIs de rendu côté serveur _(NdT: server-side rendering APIs)_ de Gatsby pour personnaliser les paramètres affectant le rendu côté serveur.

### Ressources additionnelles

- Pour un tour de tous les fichiers et répertoires usuels, voir la documentation sur la [structure d'un projet Gatsby](/docs/gatsby-project-structure/)
- Pour une liste de commandes les plus utilisées, voir la [documentation de Gatsby CLI](/docs/gatsby-cli)
- Pour avoir toutes les infos accessibles en un seul coup d'oeil, téléchargez le [pense-bête Gatsby](/docs/cheat-sheet/)

## Création automatique de pages

Gatsby transforme automatiquement les composants React qui se trouvent dans `src/pages` en pages web, qui possèdent leur propre URL.
Par exemple, les composants placés dans `src/pages/index.js` et `src/pages/about.js` serviront à créer automatiquement deux pages web: la page d'accueil du site (`/`) et la page `/about` dont les URLs ont été automatiquement générées à partir des noms de fichiers.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start)
- [Gatsby CLI](/docs/gatsby-cli) doit être installé

### Instructions

1. Créer un dossier `src/pages`, si votre site ne le contient pas déjà.
2. Ajoutez un fichier contenant un composant dans le dossier "pages":

```jsx:title=src/pages/about.js
import React from "react"

const AboutPage = () => (
  <main>
    <h1>A propos de l'auteur</h1>
    <p>Bienvenue sur mon site Gatsby.</p>
  </main>
)

export default AboutPage
```

3. Exécutez `gatsby develop` pour démarrer un serveur de développement.
4. Accédez à votre nouvelle page dans un navigateur web: `http://localhost:8000/about`

### Ressources additionnelles

- [Création et modification de pages](/docs/creating-and-modifying-pages/)

## Création de liens entre les pages

Le routage _(NdT: routing)_ de liens internes à un site Gastby repose sur le composant `<Link />`.

### Conditions préalables

- Un site Gatsby avec 2 composants de page: `index.js` et `contact.js`
- Installez [Gatsby CLI](/docs/gatsby-cli/) pour pouvoir exécuter la commande `gatsby develop`

### Instructions

1. Ouvrez le composant correspondant à la page d'accueil (`src/pages/index.js`) et importez le composant `<Link />` du module Gatsby. Ajoutez un composant `<Link />` au code JSX et assignez à la propriété `to` le chemin `"/contact/"` pour créer un lien HTML grâce aux super-pouvoirs de Gatsby:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line

export default () => (
  <main>
    <h1>Quel monde merveilleux!</h1>
    <p>
      <Link to="/contact/">Contact</Link> // highlight-line
    </p>
  </main>
)
```

2. Exécutez `gatsby develop` et naviguez vers la page d'accueil. Vous devriez avoir maintenant un lien qui vous dirigera vers la page "Contact" lorsque vous cliquez dessus!

> **Remarque**: Le composant `<Link />` de Gatsby est un wrapper autour du composant [Link de `@reach/router`](https://reach.tech/router/api/Link). Il est rendu dans le navigateur web par un élément HTML d'ancre _(NdT: HTML anchor element)_, ayant des fonctionnalités JavaScript pré-intégrées pour une performance optimale. Pour plus d'informations, consultez la [description de l'API de `<Link />`](/docs/gatsby-link/).

### Ressources additionnelles

- [Guide pour la création de Liens entre les Pages](/docs/linking-between-pages)
- [Gatsby Link API](/docs/gatsby-link)

## Création d'un composant de mise en page _(NdT: layout component)_

Il est courant d'envelopper vos pages dans un composant React de mise en page, ce qui permet de ré-utiliser dans plusieurs pages les balises _(NdT: markup)_, styles ou fonctionnalités communs.  

### Conditions préalables

- [Un site Gatsby](/docs/quick-start/)

### Instructions

1. Dans le dossier `src/components`, créez un composant de mise en page. Les composants enfants lui seront transmis en tant que props:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

2. Importez et utilisez ce composant de mise en page dans une de vos pages:

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contact</Link>
    <p>Quel monde merveilleux!</p>
  </Layout>
)
```

### Ressources additionnelles

- Pour plus d'infos sur la création des composants de mise en page, voir le [tutoriel partie 3](/tutorial/part-three/#your-first-layout-component)
- Définition de styles via les [Composants de Mise en page](/docs/layout-components/)

## Création dynamique de pages via createPage

Vous pouvez créer des pages de manière dynamique, en écrivant du code dans le fichier `gatsby-node.js` et en utilisant les fonctions fournies par Gatsby.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start)
- Un fichier `gatsby-node.js`

### Instructions

1. Dans le fichier `gatsby-node.js`, ajoutez un export de `createPages`

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. Extrayez par décomposition _(NdT: Destructure)_ l'action `createPage`, de sorte qu'elle puisse être utilisée directement pour ajouter ou obtenir des données.

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions
  // utilisez des données quelconques
  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-end
}
```

3. Effectuez une boucle sur les données et, pour chaque élément, appelez la méthode `createPage` avec comme arguments un chemin, un template et un context (données qui seront disponibles dans le pageContext des props).

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  const { createPage } = actions

  const dogData = [
    {
      name: "Fido",
      breed: "Sheltie",
    },
    {
      name: "Sparky",
      breed: "Corgi",
    },
  ]
  // highlight-start
  dogData.forEach(dog => {
    createPage({
      path: `/${dog.name}`,
      component: require.resolve(`./src/templates/dog-template.js`),
      context: { dog },
    })
  })
  // highlight-end
}
```

4. Créez un composant React pour le template que vous avez passé comme argument à `createPage`. Ce composant servira de template/modèle pour vos nouvelles pages.

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. Exécutez `gatsby develop` et visitez une de vos nouvelles pages (par exemple, `http://localhost:8000/Fido`) pour constater que les données que vous avez fournies sont bien affichées.

### Ressources additionnelles

- La section du tutoriel consacrée à la [création de pages de manière dynamique à partir de données](/tutorial/part-seven/)
- Guide consacré à [l'utilisation de Gatsby sans GraphQL](/docs/using-gatsby-without-graphql/)
- Un exemple pour cette recette est fourni dans [ce repo](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage)
