---
title: Les Données dans Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

Bienvenue dans la partie quatre du tutoriel ! Vous êtes à la moitié du chemin ! 
J'espère que vous commencez à vous sentir à l'aise 😀

## Récap de la première moitié du tutoriel

Jusqu'à maintenant, vous avez appris comment utiliser React — et combien il peut être puissant de créer _ses_ propres composants pour en faire des blocs de construction de son site web.

Vous avez également exploré les composants de style à l'aide de modules CSS.

## Que contient ce tutoriel ?

Dans les quatre prochaines parties de ce tutoriel (incluant celui-ci), vous allez découvrir la couche de données de Gatsby, qui est une des puissantes fonctionnalités de Gatsby qui vous permet entre autres de construire des sites depuis du Markdown, WordPress, Headless CMS, et d'autres types de données venant de possiblement n'importe quelle source.

**NOTE:** La couche de données de Gatsby est gérée par GraphQL. Pour un tutoriel en profondeur sur GraphQL, nous vous recommandons [Comment GraphQL](https://www.howtographql.com/).

## Données dans Gatsby

Un site web possède quatre parties : HTML, CSS, JS, et les données. La première moitié du tutoriel est basée sur les trois premiers. Maintenant commençons à apprendre à utiliser les données dans les sites Gatsby.

**Que sont les données ?**

Une réponse orientée sciences informatique serait : des données comme `"chaîne de caractères"`, entiers (`42`), objets (`{ pizza: true }`), etc.

Dans le but de travailler avec Gatsby, une réponse plus utile est "tout ce qui vit en dehors d'un composant React".

Jusqu'à maintenant, vous avez écrit du texte et ajouté des images _directement_ dans les composants.
Ce qui est un _excellent_ moyen de construire de nombreux sites web. Mais, souvent vous allez vouloir stocker _en dehors_ des composants et embarquer les données _dans_ les composants quand vous le souhaitez.

Si vous construisez un site avec WordPress (de sorte que les autres contributeurs possèdent une chouette interface pour ajouter & maintenir le contenu) et Gatsby, les _données_ pour le site (pages et articles) sont dans WordPres et vous _récupérez_ ces données, quand besoin, dans vos composants.

Les données peuvent aussi vivre dans des fichiers du type Markdown, CSV, etc. ainsi que des bases de données et APIs de toute sorte.

**La couche de données de Gatsby vous permet de récupérer les données depuis ceux-ci (ainsi que toute autre source) directement dans vos composants** — dans la forme que vous voulez.

## Utilisation de données non structurée vs GraphQL

### Ai-je besoin d'utiliser GraphQL et d'autres plugins pour récupérer des données dans des sites Gatsby ?

Absolument pas ! Vous pouvez utiliser l'API `createPages` pour récupérer les données dans un format non structuré dans des pages Gatsby directement, au lieu de passer par la couche de données de GraphQL. C'est un bon choix pour les petits sites, alors que GraphQL et les autres plugins vous permet de gagner du temps avec des sites plus complexes.

Voir le guide [Utiliser Gatsby sans GraphQL](/docs/using-gatsby-without-graphql/) pour apprendre à récupérer des données dans votre site Gatsby en utilisant l'API `createPages` et voir un site type !

### Quand utiliser les données non structurées vs GraphQL ?

Si vous êtes en train de construire un petit site, une façon efficace de le construire est de récupérer les données comme présenté dans ce guide, en utilisant l'API  `createPages`, et ensuite si le site devient plus complexe, vous modifiez votre site pour une structure plus complexe, ou si vous vous voulez transformer vos données, suivez ces étapes :

1.  Vérifier la [Bibliothèque des Plugins](/plugins/) pour voir si la source du plugin et/ou le plugin de transformation que vous souhaitez utiliser existe déjà
2.  S'ils n'existent pas encore, lisez le guide sur la [Création de plugin](/docs/creating-plugins/) et envisager de construire le vôtre !

### Comment la couche de données de Gatsby utilise GraphQL pour récupérer les données dans ses composants

<<<<<<< HEAD
=======
There are many options for loading data into React components. One of the most
popular and powerful of these is a technology called
[GraphQL](https://graphql.org/).
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097

Il existe de nombreuses options pour charger les données dans les composants React. Une des plus populaires et puissantes de celle-ci est une technologie appelée [GraphQL](http://graphql.org/).

GraphQL a été inventé chez Facebook pour aider les ingénieurs à _récupérer_ les données dans des composants

GraphQL est un **l**anguage de **q**uery (en anglais **q**uery **l**anguage, la partie _QL_ de son nom). Si vous êtes familiers avec SQL, il fonctionne d'une façon similaire. Utilisant une syntaxe spéciale, vous décrivez la donnée que vous voulez dans votre component et la donnée vous est ensuite transmise.

Gatsby utilise GraphQL pour permettre aux composants de déclarer les données dont ils ont besoin.

## Créer un nouveau site exemple

Créez un nouveau site pour cette partie du tutoriel. Vous allez créer un blog Markdown appelé "Les Pandas Mangent Beaucoup". Il est dédié à montrer les meilleures images et vidéos de pandas mangeant beaucoup de nourriture. En cours de route, vous plongerez dans les bases de GraphQL et du Markdown de Gatsby.

Ouvrez un nouveau terminal et lancez les commandes suivantes pour créer un site Gatsby dans un répertoire appelé `tutorial-part-four`. Ensuite naviguez dans le nouveau dossier :

```shell
gatsby new tutorial-part-four https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-four
```

Ensuite, installez d'autres dépendances nécessaires à la racine du projet. Vous allez utiliser le thème de la typographie "Kirkham", et vous allez essayer une librairie CSS-in-JS, ["Emotion"](https://emotion.sh/):

```shell
npm install --save gatsby-plugin-typography typography react-typography typography-theme-kirkham gatsby-plugin-emotion @emotion/core
```

Mettre en place un site similaire à ce que vous avez réalisé dans la [Partie Trois](/tutorial/part-three). Ce site aura un composant de mise en page et deux composants de page :

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
import { Link } from "gatsby"

import { rhythm } from "../utils/typography"

export default ({ children }) => (
  <div
    css={css`
      margin: 0 auto;
      max-width: 700px;
      padding: ${rhythm(2)};
      padding-top: ${rhythm(1.5)};
    `}
  >
    <Link to={`/`}>
      <h3
        css={css`
          margin-bottom: ${rhythm(2)};
          display: inline-block;
          font-style: normal;
        `}
      >
        Les Pandas Mangent Beaucoup
      </h3>
    </Link>
    <Link
      to={`/about/`}
      css={css`
        float: right;
      `}
    >
      A Propos
    </Link>
    {children}
  </div>
)
```

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>Des Pandas Extraordinaires Mangeant Des Choses</h1>
    <div>
      <img
        src="https://2.bp.blogspot.com/-BMP2l6Hwvp4/TiAxeGx4CTI/AAAAAAAAD_M/XlC_mY3SoEw/s1600/panda-group-eating-bamboo.jpg"
        alt="Groupe de pandas mangeant des bambous"
      />
    </div>
  </Layout>
)
```

```jsx:title=src/pages/about.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <h1>A Propos Des Pandas Mangent Beaucoup</h1>
    <p>
      Nous sommes le seul site fonctionnant sur votre ordinateur à afficher les meilleures photos et vidéos de pandas mangeant beaucoup de nourriture.
    </p>
  </Layout>
)
```

```javascript:title=src/utils/typography.js
import Typography from "typography"
import kirkhamTheme from "typography-theme-kirkham"

const typography = new Typography(kirkhamTheme)

export default typography
export const rhythm = typography.rhythm
```

`gatsby-config.js` (doit être à la racine de votre projet, et non pas dans src)

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
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

Ajoutez les fichiers ci-dessus et lancez `gatsby develop`, comme d'habitude, et vous devriez voir ceci :

![start](start.png)

Vous avez un autre petit site avec une mise en page et deux pages.

Maintenant, vous pouvez commencer à interroger vos données 😋

## Votre première requête GraphQL

Lors de la construction de sites, vous voudrez probablement réutiliser des données courantes -- comme le _titre du site_ par exemple. Regardez la page `/about/`. Vous allez voir le titre du site (`Pandas Eating Lots`) dans les deux composants de mise en page (l'en-tête du site) ainsi que dans le tag `<h1 />` de la page `about.js` (la page d'en-tête).

Mais que faire si vous voulez changer le titre du site à l'avenir ? Vous devrez rechercher le titre dans tous vos composants et éditer chaque occurrence. Cela est à la fois fastidieux et source d’erreurs, en particulier pour les sites plus grands et plus complexes. Au lieu de cela, vous pouvez stocker le titre dans un emplacement et référencer cet emplacement à partir d'autres fichiers; changer le titre en un seul endroit, et Gatsby va _récupérer_ votre titre mis à jour dans des fichiers qui le référencent.

La place pour ces données courantes est dans l'objet `siteMetadata` dans le fichier `gatsby-config.js`. Ajoutez votre titre de site dans le fichier `gatsby-config.js` :

```javascript:title=gatsby-config.js
module.exports = {
  // highlight-start
  siteMetadata: {
    title: `Titre depuis siteMetadata`,
  },
  // highlight-end
  plugins: [
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

Relancez le serveur de développement.

### Utilisation d'une requête de page

Maintenant, le titre du site est prêt à être interrogé; Ajoutez le dans le fichier `about.js` en utilisant une [requête de page](/docs/page-query):

```jsx:title=src/pages/about.js
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Layout from "../components/layout"

// highlight-next-line
export default ({ data }) => (
  <Layout>
    <h1>A Propos {data.site.siteMetadata.title}</h1> {/* highlight-line */}
    <p>
      Nous sommes le seul site fonctionnant sur votre ordinateur à afficher les meilleures photos et vidéos de pandas mangeant beaucoup de nourriture.
    </p>
  </Layout>
)

// highlight-start
export const query = graphql`
  query {
    site {
      siteMetadata {
        title
      }
    }
  }
`
// highlight-end
```

Ca marche! 🎉

![Page récupérant le titre depuis siteMetadata](site-metadata-title.png)

La requête GraphQL de base qui récupère le `title` dans vos changements sur le fichier `about.js` est :

```graphql:title=src/pages/about.js
{
  site {
    siteMetadata {
      title
    }
  }
}
```

> 💡 Dans la [Partie Cinq](/tutorial/part-five/#introducing-graphiql), vous allez rencontrer un outil qui vous permettra d'explorer de façon interactive les données disponibles dans GraphQL, et apprendre à écrire des requêtes comme celle ci-dessus.

Les requêtes de page vivent en dehors de la définition du composant - par convention, à la fin du fichier d'un composant de page - et sont uniquement disponibles sur les composants de page.

### Utilisation d'une StaticQuery

[StaticQuery](/docs/static-query/) est une nouvelle API introduite dans Gatsby v2 qui permet à des composants non-page (comme votre composant `layout.js`), de récupérer des données depuis des requêtes GraphQL.
Commençons par introduire la nouvelle version du hook — [`useStaticQuery`](/docs/use-static-query/).

Allez-y et apportez quelques modifications à votre fichier `src/components/layout.js` afin d'utiliser le hook `useStaticQuery` et une référence `{data.site.siteMetadata.title}` qui utilise cette donnée. Quand vous avez fini, votre fichier devrait ressembler à ceci :

```jsx:title=src/components/layout.js
import React from "react"
import { css } from "@emotion/core"
// highlight-next-line
import { useStaticQuery, Link, graphql } from "gatsby"

import { rhythm } from "../utils/typography"
// highlight-start
export default ({ children }) => {
  const data = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
          }
        }
      }
    `
  )
  return (
    // highlight-end
    <div
      css={css`
        margin: 0 auto;
        max-width: 700px;
        padding: ${rhythm(2)};
        padding-top: ${rhythm(1.5)};
      `}
    >
      <Link to={`/`}>
        <h3
          css={css`
            margin-bottom: ${rhythm(2)};
            display: inline-block;
            font-style: normal;
          `}
        >
          {data.site.siteMetadata.title} {/* highlight-line */}
        </h3>
      </Link>
      <Link
        to={`/about/`}
        css={css`
          float: right;
        `}
      >
        A Propos
      </Link>
      {children}
    </div>
    // highlight-start
  )
}
// highlight-end
```

Une autre réussite ! 🎉

![Titre de page et le titre de la mise en page récupéré depuis siteMetadata](site-metadata-two-titles.png)


Pourquoi utiliser deux requêtes différentes ici? Ces exemples étaient des introductions rapides aux types de requête, à leur formatage et à leur utilisation. Pour l'instant, garder à l'esprit que seules les pages peuvent faire des requêtes de page. Les composants non-page, comme le Layout, peuvent utiliser StaticQuery. La [Partie Sept](/tutorial/part-seven/) du tutoriel explique ceux-ci en plus grande profondeur.

Mais rétablissons le vrai titre.

Un des principes de base de Gatsby est que _les créateurs ont besoin d'une connexion immédiate avec ce qu'ils créent_ ([Chapeau bas à Bret Victor](http://blog.ezyang.com/2012/02/transcript-of-inventing-on-principle/)). En d'autres mots, quand vous faites un changement dans le code vous devriez voir directement l'effet de ce changement. Vous manipulez un champ d'entrée de Gatsby et vous devriez voir le contenu s'afficher sur l'écran.

Donc presque partout, les changements que vous faites prendront effet. Editez encore le fichier `gatsby-config.js`, cette fois changez le `title` pour "Les Pandas Mangent Beaucoup". Les changements devraient prendre place rapidement dans les pages de votre site.

![Les deux titres disent que Les Pandas Mangent Beaucoup](pandas-eating-lots-titles.png)

## Qu'est-ce qui arrive ensuite ?

Ensuite, vous apprendrez à extraire des données de votre site Gatsby en utilisant GraphQL avec des plugins dans la [Partie Cinq](/tutorial/part-five/) du tutoriel.
