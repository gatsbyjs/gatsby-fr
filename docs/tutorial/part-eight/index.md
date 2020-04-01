---
title: Préparation d'un site pour la mise en ligne
typora-copy-images-to: ./
disableTableOfContents: true
---

Ouah! Vous avez fait un long chemin! Vous avez appris à:

- créer de nouveaux sites Gatsby
- créer des pages et des composants
- styliser des composants
- ajouter des plugins à un site
- sourcer et transformer des données
- utiliser GraphQL pour requêter les données des pages
- créer des pages à partir de vos données

Dans cette dernière section, vous allez découvrir quelques étapes essentielles pour préparer un site à la mise en ligne en introduisant un puissant outil de diagnostic de site appelé [Lighthouse](https://developers.google.com/web/tools/lighthouse/). En cours de route, nous vous présenterons d'autres plugins que vous voudrez souvent utiliser dans vos sites Gatsby.

## Audit avec Lighthouse

Citation tirée du [site web Lighthouse](https://developers.google.com/web/tools/lighthouse/):

> Lighthouse est un outil automatisé open-source destiné à améliorer la qualité des pages web.
> Vous pouvez le lancer sur n'importe quelle page web, publique ou nécessitant une authentification. Il comporte des audits de performance, d'accessibilité, des applications web progressives (PWA), etc.

Lighthouse est inclus dans Chrome DevTools. L'exécution de son audit -- puis la correction des erreurs qu'il trouve et la mise en oeuvre des améliorations qu'il suggère -- est un excellent moyen de préparer votre site à la mise en ligne. Cela vous permet de vous assurer que votre site est aussi rapide et accessible que possible.

Essayez-le !

Tout d'abord, vous devez créer une version de production de votre site Gatsby. Le serveur de développement Gatsby est conçu pour rendre le développement rapide; mais le site qu'il génère, bien que ressemblant de près à une version en production du site, n'est pas aussi optimisé.

### ✋ Créer un "build" de production

1. Arrêtez le serveur de développement (s'il est toujours en cours d'exécution) et exécutez la commande suivante:

```shell
gatsby build
```

> 💡Comme vous l'avez appris dans la [partie 1](/tutorial/part-one/), cette méthode permet de concevoir votre site en production et générer les fichiers statiques dans le répertoire `public`.

2. Visualisez le site de production en local.
   Exécutez:

```shell
gatsby serve
```

Une fois que cela aura commencé, vous pourrez consulter votre site à l'adresse suivante `http://localhost:9000`.

### Effectuer un audit Lighthouse

Vous allez maintenant faire votre premier test Lighthouse.

1. Si vous ne l'avez pas encore fait, ouvrez le site en mode Chrome Incognito afin qu'aucune extension ne vienne perturber le test. Ensuite, ouvrez Chrome DevTools.

2. Cliquez sur l'onglet "Audits" où vous verrez un écran qui ressemble à:

![Lighthouse audit start](./lighthouse-audit.png)

3. Cliquez sur "Effectuer un audit...". (Tous les types d'audit disponibles doivent être sélectionnés par défaut). Cliquez ensuite sur "Exécuter un audit". (Il vous faudra alors environ une minute pour exécuter l'audit). Une fois l'audit terminé, vous devriez voir des résultats qui ressemblent à ceci :

![Lighthouse audit results](./lighthouse-audit-results.png)

Comme vous pouvez le constater, les performances de Gatsby sont excellentes dès le départ, mais il vous manque certains éléments pour la PWA, l'accessibilité, les bonnes pratiques et le SEO qui amélioreront vos scores (et qui rendront votre site beaucoup plus convivial pour les visiteurs et les moteurs de recherche).

## Ajouter un fichier manifest

On dirait que vous avez un score assez faible dans la catégorie "Progressive Web App". Voyons cela.

Mais d'abord, qu'est-ce qu'une PWA exactement ?

Il s'agit de sites web classiques qui tirent parti des fonctionnalités d'un navigateur moderne pour enrichir l'expérience web avec les caractéristiques et les avantages des applications. Consultez [l'aperçu Google](https://developers.google.com/web/progressive-web-apps/) pour savoir ce qui caractérise une expérience PWA.

La présence d'un manifeste d'application web est l'une des trois [conditions de base généralement acceptées pour une PWA](https://alistapart.com/article/yes-that-web-project-should-be-a-pwa#section1).

Citation de [Google](https://developers.google.com/web/fundamentals/web-app-manifest/):

> Le manifeste d'application web est un simple fichier JSON qui renseigne le navigateur à propos de votre application web et la manière dont elle doit se comporter lorsqu'elle est "installée" sur l'appareil mobile ou le bureau de l'utilisateur.

[Le plugin manifest de Gatsby](/packages/gatsby-plugin-manifest/) configure Gatsby pour créer un fichier `manifest.webmanifest` à chaque phase de construction de site.

### ✋Utilisation de `gatsby-plugin-manifest`

1. Installez le plugin:

```shell
npm install --save gatsby-plugin-manifest
```

2. Ajoutez une favicon pour votre application sous `src/images/icon.png`. Pour les besoins de ce tutoriel, vous pouvez utiliser [cet exemple d'icône](https://raw.githubusercontent.com/gatsbyjs/gatsby/master/docs/tutorial/part-eight/icon.png), si vous n'en avez pas. L'icône est nécessaire pour construire toutes les images du manifeste. Pour plus d'informations, consultez la documentation de [`gatsby-plugin-manifest`](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-plugin-manifest/README.md).

3. Ajoutez le plugin au tableau `plugins` dans votre fichier `gatsby-config.js`

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png` // This path is relative to the root of the site.
      }
    }
  ];
}
```

C'est tout ce dont vous avez besoin pour commencer à ajouter un manifeste web à un site Gatsby. L'exemple donné reflète une configuration de base -- Consultez la [référence du plugin](/packages/gatsby-plugin-manifest/?=gatsby-plugin-manifest#automatic-mode) pour plus d'options.

## Ajouter un support hors ligne

Une autre condition pour qu'un site web puisse être qualifié de PWA est l'utilisation d'un [service worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API).
Ce dernier travaille en arrière-plan et décide de diffuser du contenu en réseau ou en cache en fonction de la connectivité, ce qui permet une expérience hors ligne maîtrisée et fluide.

[Le plugin offline de Gatsby](/packages/gatsby-plugin-offline/) permet à un site Gatsby de fonctionner hors ligne et de mieux répondre aux dysfonctionnements du réseau en créant un service worker pour votre site.

### ✋ Utilisation de `gatsby-plugin-offline`

1. Installez le plugin:

```shell
npm install --save gatsby-plugin-offline
```

2. Ajoutez le plugin au tableau des `plugins` dans votre fichier `gatsby-config.js`.

```javascript:title=gatsby-config.js
{
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png` // This path is relative to the root of the site.
      }
    },
    // highlight-next-line
    `gatsby-plugin-offline`
  ];
}
```

C'est tout ce dont vous avez besoin pour commencer à travailler avec des service workers chez Gatsby.

> 💡 Le plugin hors ligne doit être listé après le plugin manifest afin que le plugin hors ligne puisse mettre en cache le `manifest.webmanifest` créé.

## Ajouter des métadonnées de page

L'ajout de métadonnées aux pages (telles qu'un titre ou une description) est essentiel pour aider les moteurs de recherche comme Google à comprendre votre contenu et à décider quand le faire apparaître dans les résultats de recherche.

[React Helmet](https://github.com/nfl/react-helmet) est un logiciel qui fournit une interface de composant React pour vous permettre de gérer votre [en-tête de document](https://developer.mozilla.org/fr/docs/Web/HTML/Element/head).

Le plugin [react helmet](/packages/gatsby-plugin-react-helmet/) de Gatsby fournit un support pour les données de rendu du serveur ajoutées avec React Helmet. En utilisant le plugin, les attributs que vous ajoutez à React Helmet seront ajoutés aux pages HTML statiques que Gatsby construit.

### ✋ Utilisation de `React Helmet` et `gatsby-plugin-react-helmet`

1. Installez les deux paquets:

```shell
npm install --save gatsby-plugin-react-helmet react-helmet
```

2. Assurez-vous d'avoir une `description` et un `auteur` configurés dans votre objet `siteMetadata`. Ajoutez également le plugin `gatsby-plugin-react-helmet` au tableau `plugins` dans votre fichier `gatsby-config.js`.

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Pandas Eating Lots`,
    // highlight-start
    description: `A simple description about pandas eating lots...`,
    author: `gatsbyjs`
    // highlight-end
  },
  plugins: [
    {
      resolve: `gatsby-plugin-manifest`,
      options: {
        name: `GatsbyJS`,
        short_name: `GatsbyJS`,
        start_url: `/`,
        background_color: `#6b37bf`,
        theme_color: `#6b37bf`,
        // Enables "Add to Homescreen" prompt and disables browser UI (including back button)
        // see https://developers.google.com/web/fundamentals/web-app-manifest/#display
        display: `standalone`,
        icon: `src/images/icon.png` // This path is relative to the root of the site.
      }
    },
    `gatsby-plugin-offline`,
    // highlight-next-line
    `gatsby-plugin-react-helmet`
  ]
};
```

3. Dans le répertoire `src/components`, créez un fichier appelé `seo.js` et ajoutez ce qui suit :

```jsx:title=src/components/seo.js
import React from 'react';
import PropTypes from 'prop-types';
import Helmet from 'react-helmet';
import { useStaticQuery, graphql } from 'gatsby';

function SEO({ description, lang, meta, title }) {
  const { site } = useStaticQuery(
    graphql`
      query {
        site {
          siteMetadata {
            title
            description
            author
          }
        }
      }
    `
  );

  const metaDescription = description || site.siteMetadata.description;

  return (
    <Helmet
      htmlAttributes={{
        lang
      }}
      title={title}
      titleTemplate={`%s | ${site.siteMetadata.title}`}
      meta={[
        {
          name: `description`,
          content: metaDescription
        },
        {
          property: `og:title`,
          content: title
        },
        {
          property: `og:description`,
          content: metaDescription
        },
        {
          property: `og:type`,
          content: `website`
        },
        {
          name: `twitter:card`,
          content: `summary`
        },
        {
          name: `twitter:creator`,
          content: site.siteMetadata.author
        },
        {
          name: `twitter:title`,
          content: title
        },
        {
          name: `twitter:description`,
          content: metaDescription
        }
      ].concat(meta)}
    />
  );
}

SEO.defaultProps = {
  lang: `en`,
  meta: [],
  description: ``
};

SEO.propTypes = {
  description: PropTypes.string,
  lang: PropTypes.string,
  meta: PropTypes.arrayOf(PropTypes.object),
  title: PropTypes.string.isRequired
};

export default SEO;
```

Le code ci-dessus définit les valeurs par défaut pour vos balises de métadonnées les plus courantes et vous fournit un composant `<SEO>` avec lequel vous pourrez travailler pour le reste de votre projet. Plutôt cool, non ?

4. Maintenant, vous pouvez utiliser le composant `<SEO>` dans vos modèles et pages et lui passer des props. Par example, ajoutez-le à votre modèle `blog-post.js` comme ceci :

```jsx:title=src/templates/blog-post.js
import React from 'react';
import { graphql } from 'gatsby';
import Layout from '../components/layout';
// highlight-next-line
import SEO from '../components/seo';

export default ({ data }) => {
  const post = data.markdownRemark;
  return (
    <Layout>
      // highlight-start
      <SEO title={post.frontmatter.title} description={post.excerpt} />
      // highlight-end
      <div>
        <h1>{post.frontmatter.title}</h1>
        <div dangerouslySetInnerHTML={{ __html: post.html }} />
      </div>
    </Layout>
  );
};

export const query = graphql`
  query($slug: String!) {
    markdownRemark(fields: { slug: { eq: $slug } }) {
      html
      frontmatter {
        title
      }
      // highlight-next-line
      excerpt
    }
  }
`;
```

L'exemple ci-dessus est basé sur le [Gatsby Starter Blog](/starters/gatsbyjs/gatsby-starter-blog/). En passant des props au composant `<SEO>`, vous pouvez modifier dynamiquement les métadonnées d'un article de blog. Dans ce cas, le `titre` et l'`extrait` de l'article de blog (s'il existe dans le fichier markdown de l'article) seront utilisés à la place des propriétés par défaut des `siteMetadata` dans votre fichier `gatsby-config.js`.

Maintenant, si vous lancez l'audit Lighthouse comme indiqué ci-dessus, vous devriez vous approcher--d' une parfaite-- note de 100!

> 💡 Pour plus d'informations et d'exemples, consultez [Ajout d'un composant SEO](/docs/add-seo-component/) et la [Documentation React Helmet](https://github.com/nfl/react-helmet#example)!

## Amélioration

Dans cette section, nous vous avons montré quelques outils spécifiques à Gatsby pour améliorer les performances de votre site et préparer son lancement.

Lighthouse est un excellent outil pour l'amélioration et la connaissance du site -- Continuez à consulter les commentaires détaillés qu'il fournit et continuez à améliorer votre site !

## Prochaines étapes

### Documentation officielle

- [Documentation Officielle](https://www.gatsbyjs.org/docs/): Consultez notre documentation officielle pour les rubriques _[Quick Start](https://www.gatsbyjs.org/docs/quick-start/)_, _[Guides Détaillés](https://www.gatsbyjs.org/docs/preparing-your-environment/)_, _[Références API](https://www.gatsbyjs.org/docs/gatsby-link/)_, et bien plus.

### Plugins officiels

- [Official Plugins](https://github.com/gatsbyjs/gatsby/tree/master/packages): La liste complète de tous les plugins officiels maintenus par Gatsby.

### Starters officiels

1.  [Gatsby's Default Starter](https://github.com/gatsbyjs/gatsby-starter-default): Démarrez votre projet avec ce boilerplate par défaut. Ce starter est livré avec les principaux fichiers de configuration de Gatsby dont vous pourriez avoir besoin. _[example](https://gatsbyjs.github.io/gatsby-starter-default/)_
2.  [Gatsby's Blog Starter](https://github.com/gatsbyjs/gatsby-starter-blog): Gatsby starter pour créer un blog génial et rapide comme l'éclair. _[example](https://gatsbyjs.github.io/gatsby-starter-blog/)_
3.  [Gatsby's Hello-World Starter](https://github.com/gatsbyjs/gatsby-starter-hello-world): Gatsby Starter avec le strict nécessaire pour un site Gatsby. _[example](https://gatsby-starter-hello-world-demo.netlify.com/)_

## C'est tout, les amis

Enfin, pas tout à fait; juste pour ce tutoriel. Il y a des [Tutoriels Supplémentaires](/tutorial/additional-tutorials/) à consulter pour des cas d'usage plus guidés.

Ce n'est que le début. Continuez !

- Vous avez construit quelque chose de cool ? Partagez-le sur Twitter, tag [#buildwithgatsby](https://twitter.com/search?q=%23buildwithgatsby), et [@mentionnez-nous](https://twitter.com/gatsbyjs)!
- Avez-vous écrit un article de blog sympa sur ce que vous avez appris ? Partagez cela aussi !
- Contribuez ! Faites un tour sur les [open issues](https://github.com/gatsbyjs/gatsby/issues?q=is%3Aissue+is%3Aopen+label%3A%22help+wanted%22) sur le repo de Gatsby et [devenez un contributeur](/contributing/how-to-contribute/).

Consultez la documentation sur ["comment contribuer"](/contributing/how-to-contribute/) pour encore plus d'idées.

Nous sommes impatients de voir ce que vous ferez 😄.
