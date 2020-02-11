---
title: Les plugins source
typora-copy-images-to: ./
disableTableOfContents: true
---

> Ce tutoriel fait partie d'une série concernant la couche de données de Gatsby. Assurez-vous d'avoir suivi la [partie 4](/tutorial/part-four/) avant de poursuivre ici.

## Que contient ce tutoriel ?

Dans ce tutoriel, vous apprendrez comment intégrer des données dans votre site Gatsby en utilisant GraphQL et des plugins source. Avant de vous pencher sur ces plugins cependant, vous devez apprendre à vous servir d'une chose qui s'appelle GraphiQL, un outil qui vous aide à structurer vos requêtes correctement.

## Introduction à GraphiQL

GraphiQL est l'environnement de développement intégré (EDI) de GraphQL. C'est un outil puissant (et en tout point génial) que vous serez souvent amené à utiliser en construisant des sites web Gatsby.

<<<<<<< HEAD
Vous pouvez y accéder lorsque votre serveur de développement est lancé - normalement ici
<http://localhost:8000/___graphql>.
=======
You can access it when your site's development server is running—normally at
`http://localhost:8000/___graphql`.
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="/graphiql-explore.mp4"></source>
  <p>Votre navigateur ne prend pas en charge l'élément HTML5 "vidéo"</p>
</video>

Faites un tour dans le "type" `Site` intégré et regardez quels champs y sont disponibles -- dont l'objet `siteMetadata` que nous avons interrogé plus tôt. Essayez d'ouvrir GraphiQL et de jouer avec vos données ! Appuyez sur <kbd>Ctrl + Espace</kbd> (ou utilisez <kbd>Maj + Espace</kbd> comme raccourci clavier alternatif) pour faire apparaître la fenêtre d'autocomplétion et <kbd>Ctrl + Entrer</kbd> pour exécuter votre requête. Vous utiliserez beaucoup plus GraphiQL d'ici à la fin de ce tutoriel.

## Utiliser l'explorateur de GraphiQL

L'explorateur GraphiQL vous permet de construire de façon interactive des requêtes complètes en cliquant à travers les champs et les entrées disponibles, sans le processus rébarbartif de taper ces requêtes à la main.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-build-a-graphql-query-using-gatsby-s-graphiql-explorer"
  lessonTitle="Build a GraphQL Query using Gatsby’s GraphiQL Explorer"
/>

## Plugins source

Les données des sites Gatsby peuvent venir de n'importe où : APIs, base de données, CMS, fichiers locaux, etc.

Les plugins source récupèrent les données depuis leur source. Par exemple, le plugin source "filesystem" sait comment récupérer des données depuis le système de fichier. Le plugin WordPress sait comment récupérer des données depuis l'API de WordPress.

Ajoutez [`gatsby-source-filesystem`](/packages/gatsby-source-filesystem/) et découvrez son fonctionnement.

Premièrement, installez le plugin à la racine de votre projet :

```shell
npm install --save gatsby-source-filesystem
```

Ajoutez-le ensuite à votre `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
  },
  plugins: [
    // highlight-start
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `src`,
        path: `${__dirname}/src/`,
      },
    },
    // highlight-end
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

Sauvegardez et redémarrez le serveur de développement de Gatsby. Ensuite, ouvrez à nouveau GraphiQL.

Dans le panneau de l'explorateur, vous verrez `allFile` et `file` disponible à la sélection :

![graphiql-filesystem](graphiql-filesystem.png)

Cliquez sur le menu déroulant `allFile`. Placez votre curseur après `allFile` dans la zone de requête et pressez ensuite <kbd>Ctrl + Entrer</kbd>. Cela va préremplir une requête sur l'`id` de chaque fichier. Appuyez sur "Play" pour exécuter la requête.

![filesystem-query](filesystem-query.png)

Dans le panneau de l'Explorateur, le champs `id` a été automatiquement sélectionné. Vous pouvez sélectionner plus de champs
en cochant les cases correspondantes à chaque champ. Appuyez sur "Play" pour exécuter une nouvelle fois la requête, avec les nouveaux champs :

![filesystem-explorer-options](filesystem-explorer-options.png)

Sinon, vous pouvez ajouter des champs en utilisant le raccourci d'autocomplétion (<kbd>Ctrl + Espace</kbd>). Ce qui vous affichera les champs utilisables dans votre requête pour les noeuds `File`.

![filesystem-autocomplete](filesystem-autocomplete.png)

<<<<<<< HEAD
Essayez d'ajouter de multiples champs à votre requête, en appuyant sur <kbd>Ctrl + Entrer</kbd> pour exécuter à nouveau la requête. Vous verrez les résultats mis à jour de la requête :
=======
Try adding a number of fields to your query, press <kbd>Ctrl + Enter</kbd>
each time to re-run the query. You'll see the updated query results:
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

![allfile-query](allfile-query.png)

Le résultat est un array de noeuds `File` (noeud est le drôle de nom donné à un objet dans un "graph"). Chaque noeud / objet `File` contient les champs que vous avez demandés.

## Construire une page avec une requête GraphQL

La construction de nouvelles pages dans Gatsby commence souvent dans GraphiQL. Vous commencez par schématiser votre requête de données en jouant dans GraphiQL puis en la copiant dans un composant de page React pour commencer à créer votre UI.

Essayez ceci.

Créez un nouveau fichier dans `src/pages/my-files.js` avec la requête GraphQL `allFile` que vous venez tout juste de créer :

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data) // highlight-line
  return (
    <Layout>
      <div>Hello world</div>
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

La ligne `console.log(data)` est mise en surbrillance ci-dessus. Il est souvent utile quand vous êtes en train de créer un nouveau composant, d'afficher dans la console les données que vous obtenez de votre requête GraphQL de sorte que vous puissiez explorer les données dans la console de votre navigateur tout en créant votre UI.

Si vous visitez la nouvelle page `/my-files/` et ouvrez la console de votre navigateur, vous devriez voir quelque chose comme :

![data-in-console](data-in-console.png)

La forme de vos données correspond à la forme de votre requête GraphQL.

Ajouter un peu de code à votre composant pour afficher les données de File.

```jsx:title=src/pages/my-files.js
import React from "react"
import { graphql } from "gatsby"
import Layout from "../components/layout"

export default ({ data }) => {
  console.log(data)
  return (
    <Layout>
      {/* highlight-start */}
      <div>
        <h1>My Site's Files</h1>
        <table>
          <thead>
            <tr>
              <th>relativePath</th>
              <th>prettySize</th>
              <th>extension</th>
              <th>birthTime</th>
            </tr>
          </thead>
          <tbody>
            {data.allFile.edges.map(({ node }, index) => (
              <tr key={index}>
                <td>{node.relativePath}</td>
                <td>{node.prettySize}</td>
                <td>{node.extension}</td>
                <td>{node.birthTime}</td>
              </tr>
            ))}
          </tbody>
        </table>
      </div>
      {/* highlight-end */}
    </Layout>
  )
}

export const query = graphql`
  query {
    allFile {
      edges {
        node {
          relativePath
          prettySize
          extension
          birthTime(fromNow: true)
        }
      }
    }
  }
`
```

<<<<<<< HEAD
Visitez maintenant [http://localhost:8000/my-files](http://localhost:8000/my-files)… 😲
=======
And now visit `http://localhost:8000/my-files`… 😲
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

![my-files-page](my-files-page.png)

## Que vient-il ensuite ?

Maintenant vous avez appris comment les plugins source apportent leurs données _dans_ le système de données de Gatsby. Dans le prochain tutoriel, vous apprendrez comment les plugins transformateurs _transforment_ le contenu brut apporté par les plugins source. La combinaison de plugins source et transformateurs peut se charger de toutes les transformations de données dont vous pourriez avoir besoin en construisant un site Gatsby. Apprenez-en plus à propos des plugins transformateurs dans la [partie six de ce tutoriel](/tutorial/part-six/).
