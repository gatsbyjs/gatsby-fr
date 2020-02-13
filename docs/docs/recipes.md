---
titre: Recettes
tableauOfDepthContents: 2
---

<!-- Template de base pour une recette Gatsby:

## Tâche à accomplir
Une ou deux phrases à propos du sujet. Plus cela est concis et ciblé, meilleur c'est !

### Conditions préalables
- Exigences système/version
- Tout ce qui est nécessaire pour mettre en place la tâche
- Y compris la création de comptes sur d'autres sites, comme Netlify
- Voir [docs templates](/docs/docs-templates/) pour les conseils de mise en forme

### Instructions
Instructions étape par étape. Chaque étape doit être reproductible et précise. Tout ce qui n'est pas essentiel à la tâche doit être omis.

#### Exemple en live (facultatif)
Un exemple en live peut être compliqué en fonction de la nature de la recette, auquel cas il est préférable de ne pas en faire.

### Ressources complémentaires
- Tutoriels
- Pages de documentation
- Plugin READMEs
- etc.

Voir [docs templates](/docs/docs-templates/) dans les docs contributives pour obtenir plus d'aide.
-->

A la recherche d’un juste milieu entre les [tutoriels complets](/tutorial/) et étudier ligne à ligne les [docs](/docs/) ? Voici un résumé des recettes pour construire des choses, à la sauce Gatsby.

## [1. Pages et mises en page](/docs/recipes/pages-layouts)

Ajoutez des pages à votre site Gatsby, et utilisez des mises en page pour gérer l'affichage des éléments communs sur ces pages.

- [Structure du project](/docs/recipes/pages-layouts#project-structure)
- [Créer des pages automatiquement](/docs/recipes/pages-layouts#creating-pages-automatically)
- [La navigation entre les pages](/docs/recipes/pages-layouts#linking-between-pages)
- [Créer un composant de mise en page](/docs/recipes/pages-layouts#creating-a-layout-component)
- [Créer des pages programmaticallement avec createPage](/docs/recipes/pages-layouts#creating-pages-programmatically-with-createpage)

## [2. La mise en forme avec CSS](/docs/recipes/styling-css)

Il y a énormément de façons de mettre en forme votre site ; Gatsby fonctionne avec presque toutes les options possibles grâce aux plugins officiel et à ceux de la communauté.

- [Utiliser un fichier CSS global sans composant de mise en page](/docs/recipes/styling-css#using-global-css-files-without-a-layout-component)
- [Utiliser un fichier CSS global dans un composant de mise en page](/docs/recipes/styling-css#using-global-styles-in-a-layout-component)
- [Utiliser Styled Components](/docs/recipes/styling-css#using-styled-components)
- [Utiliser CSS Modules](/docs/recipes/styling-css#using-css-modules)
- [Utiliser Sass/SCSS](/docs/recipes/styling-css#using-sassscss)
- [Ajouter une police d'écriture locale](/docs/recipes/styling-css#adding-a-local-font)
- [Utiliser Emotion](/docs/recipes/styling-css#using-emotion)
- [Utiliser Google Fonts](/docs/recipes/styling-css#using-google-fonts)

## [3. Travailler avec les plugins de démarrage](/docs/recipes/working-with-starters)

[Les plugins de démarrage](/docs/starters/) sont des sites Gatsby génériques maintenus officiellement ou par la communauté.

- [Utiliser un plugin de démarrage](/docs/recipes/working-with-starters#using-a-starter)

## [4. Travailler avec les thèmes](/docs/recipes/working-with-themes)

Un thème Gatsby vous permet de centraliser l'apparence de votre site. Vous pouvez mettre à jour un thème, composer plusieurs thèmes ensemble, et même rapidement changer de thème parmis les thèmes compatible.

- [Créer un nouveau site en utilisant un thème](/docs/recipes/working-with-themes#creating-a-new-site-using-a-theme)
- [Créer un nouveau site en thème d'un plugins de démarrage](/docs/recipes/working-with-themes#creating-a-new-site-using-a-theme-starter)
- [Coder un nouveau thème](/docs/recipes/working-with-themes#building-a-new-theme)

## [5. Récupérer les données](/docs/recipes/sourcing-data)

Pull data from multiple locations, like the filesystem or database, into your Gatsby site.

<<<<<<< HEAD
3. Maintenant, importez `Img` à partir de `gatsby-image`, et `graphql` à partir de `gatsby` dans le template du composant, écrire une [pageQuery](/docs/page-query/) pour obtenir des données d'image en utilisant le `slug` et transmettre ces données au composant `<Img />`
=======
- [Adding data to GraphQL](/docs/recipes/sourcing-data#adding-data-to-graphql)
- [Sourcing Markdown data for blog posts and pages with GraphQL](/docs/recipes/sourcing-data#sourcing-markdown-data-for-blog-posts-and-pages-with-graphql)
- [Sourcing from WordPress](/docs/recipes/sourcing-data#sourcing-from-wordpress)
- [Sourcing data from Contentful](/docs/recipes/sourcing-data#sourcing-data-from-contentful)
- [Pulling data from an external source and creating pages without GraphQL](/docs/recipes/sourcing-data#pulling-data-from-an-external-source-and-creating-pages-without-graphql)
- [Sourcing content from Drupal](/docs/recipes/sourcing-data#sourcing-content-from-drupal)
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

## [6. Querying data](/docs/recipes/querying-data)

Gatsby lets you access your data across all sources using a single GraphQL interface.

- [Querying data with a Page Query](/docs/recipes/querying-data#querying-data-with-a-page-query)
- [Querying data with the StaticQuery Component](/docs/recipes/querying-data#querying-data-with-the-staticquery-component)
- [Querying data with the useStaticQuery hook](/docs/recipes/querying-data/#querying-data-with-the-usestaticquery-hook)
- [Limiting with GraphQL](/docs/recipes/querying-data#limiting-with-graphql)
- [Sorting with GraphQL](/docs/recipes/querying-data#sorting-with-graphql)
- [Filtering with GraphQL](/docs/recipes/querying-data#filtering-with-graphql)
- [GraphQL Query Aliases](/docs/recipes/querying-data#graphql-query-aliases)
- [GraphQL Query Fragments](/docs/recipes/querying-data#graphql-query-fragments)
- [Querying data client-side with fetch](/docs/recipes/querying-data#querying-data-client-side-with-fetch)

<<<<<<< HEAD
4. Exécutez `gatsby develop`, qui générera des images pour les fichiers accessibles de votre dossier

#### Ressources complémentaires

- [Exemple de repo utilisant cette recette](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Images avec frontmatter](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [Utilisation de Gatsby Image](/docs/using-gatsby-image)
- [En savoir plus sur le travail avec des images dans Gatsby](/docs/working-with-images/)

## 8. Transformer les données
=======
## [7. Working with images](/docs/recipes/working-with-images)

Access images as static resources, or automate the process of optimizing them through powerful plugins.

- [Import an image into a component with webpack](/docs/recipes/working-with-images#import-an-image-into-a-component-with-webpack)
- [Reference an image from the static folder](/docs/recipes/working-with-images#reference-an-image-from-the-static-folder)
- [Optimizing and querying local images with gatsby-image](/docs/recipes/working-with-images#optimizing-and-querying-local-images-with-gatsby-image)
- [Optimizing and querying images in post frontmatter with gatsby-image](/docs/recipes/working-with-images#optimizing-and-querying-images-in-post-frontmatter-with-gatsby-image)

## [8. Transforming data](/docs/recipes/transforming-data)
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

La transformation des données dans Gatsby est axée sur l’utilisation de plugins. Les plugins de transformation prennent les données récupérées à l'aide de plugins sources et les transforment en quelque chose de plus utilisable (par exemple le JSON en objets JavaScript et plus encore)

<<<<<<< HEAD
### Transformer du Markdown en HTML

Le plugin `gatsby-transformer-remark` peut transformer les fichiers Markdown en HTML

#### Les prérequis

- Un site Gatsby avec `gatsby-config.js` et une page `index.js`
- Un fichier Markdown enregistré dans votre répertoire `src`
- Un plugin source installé, tel que `gatsby-source-filesystem`
- Le plugin `gatsby-transformer-remark` installé

#### Instructions

1. Ajoutez le plugin de transformation dans votre `gatsby-config.js` :

```js:title=gatsby-config.js
plugins: [
  // not shown: gatsby-source-filesystem for creating nodes to transform
  `gatsby-transformer-remark`
],
```

2. Ajoutez une requête GraphQL au fichier `index.js` de votre site Gatsby pour récupérer les nœuds `MarkdownRemark` :

```jsx:title=src/pages/index.js
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

3. Redémarrez le serveur de développement et ouvrir GraphiQL à `http://localhost:8000/___graphql`. Explorez les champs disponibles sur le nœud `MarkdownRemark`

#### Ressources complémentaires

- [Tutoriel sur la transformation de Markdown en HTML](/tutorial/part-six/#transformer-plugins) en utilisant `gatsby-transformer-remark`
- Parcourir les plugins de transformation disponibles dans la [bibliothèque de plugins de Gatsby](/plugins/?=transformer)

## 9. Déploiement de votre site
=======
- [Transforming Markdown into HTML](/docs/recipes/transforming-data#transforming-markdown-into-html)

## [9. Deploying your site](/docs/recipes/deploying-your-site)
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

Showtime ! Une fois que vous êtes content de votre site, vous êtes prêt à l’envoyer sur la toile !

<<<<<<< HEAD
### Préparation au déploiement

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Le [Gatsby CLI](/docs/gatsby-cli) installé

#### Instructions

1. Arrêtez votre serveur de développement s'il est en cours d'exécution (`Ctrl + C` depuis votre Terminal dans la plupart des cas)

2. Pour obtenir un chemin standard du site à la racine (`/`), exécutez `gatsby build` en utilisant le Gatsby CLI depuis votre Terminal. Les fichiers intégrés seront désormais dans le dossier `public`

```shell
gatsby build
```

3. Pour inclure un chemin vers le site autre que `/` (tel que `/site-name/`), fixez un préfixe de chemin en ajoutant ce qui suit à votre `gatsby-config.js` et en remplaçant `yourpathprefix` par le préfixe que vous avez choisi :

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

Il y a plusieurs raisons pour lesquelles vous voudriez procéder ainsi - par exemple, l'hébergement d'un blog construit avec Gatsby sur un domaine avec un autre site non construit sur Gatsby. Le site principal serait direct vers `example.com` et le site de Gatsby avec un chemin préfixé pourrait vivre à `example.com/blog`

4. Avec un préfixe de chemin configuré dans `gatsby-config.js`, exécutez `gatsby build` avec le flag `--prefix-paths` pour ajouter automatiquement le préfixe au début de toutes les URL du site de Gatsby et les tags ‘<Link>’

```shell
gatsby build --prefix-paths
```

5. Assurez-vous que votre site est le même lors de l'exécution de `gatsby build` que celui apparaissant en local en exécutant `gatsby develop`. En exécutant `gatsby serve` lorsque vous construisez votre site, vous pouvez tester (et déboguer si nécessaire) le produit fini avant de le déployer en direct.

```shell
gatsby build && gatsby serve
```

#### Ressources complémentaires

- Suivez la construction et le déploiement d'un site d'exemple dans [tutoriel partie un](/tutorial/part-one/#deploying-a-gatsby-site)
- En savoir plus sur [l'optimisation des performances](/docs/performance/)
- En savoir plus sur [d'autres sujets liés au déploiement](/docs/preparing-for-deployment/)
- Consultez la [doc liée aux déploiements](/docs/deploying-and-hosting/) pour des plates-formes d'hébergement spécifiques

### Déploiement sur Netlify

Utilisez [`netlify-cli`](https://www.netlify.com/docs/cli/) pour déployer votre application Gatsby sans quitter l'interface de votre Terminal

#### Les prérequis

- Un [site Gatsby](/docs/quick-start) avec un seul composant `index.js`
- Le package [netlify-cli](https://www.npmjs.com/package/netlify-cli) installé
- Le [Gatsby CLI](/docs/gatsby-cli) installé

#### Instructions

1. Construisez votre application Gatsby en utilisant `gatsby build`

2. Connectez-vous à Netlify à l'aide de `netlify login`

3. Exécutez la commande `netlify init`. Sélectionnez l'option « Créer et configurer un nouveau site ».

4. Choisissez un nom de site Web personnalisé si vous le souhaitez, ou appuyez sur entrer pour en recevoir un au hasard.

5. Choisissez votre [Team](https://www.netlify.com/docs/teams/)

6. Modifiez le chemin de déploiement vers le `public/`

7. Assurez-vous que tout semble bien avant de déployer en utilisant `netlify deploy --prod`

#### Ressources complémentaires

- [Hébergement sur Netlify](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

### Déploiement sur ZEIT Now

Utilisez [Now CLI](https://zeit.co/download) pour déployer votre application Gatsby sans quitter l’interface de votre Terminal

#### Les prérequis

- Un compte [ZEIT Now](https://zeit.co/signup)
- Un [site Gatsby](/docs/quick-start) avec un seul composant `index.js`
- Le package [Now CLI](https://zeit.co/download) installé
- [Gatsby CLI](/docs/gatsby-cli) installé

#### Instructions

1. Connectez-vous à Now CLI en utilisant `now login`

2. Allez dans le dossier de votre application Gatsby.js dans le Terminal si vous n'y êtes pas déjà

3. Exécutez `now` pour le déployer

#### Ressources complémentaires

- [Déploiement sur ZEIT Now](/docs/deploying-to-zeit-now/)
=======
- [Preparing for deployment](/docs/recipes/deploying-your-site#preparing-for-deployment)
- [Deploying to Netlify](/docs/recipes/deploying-your-site#deploying-to-netlify)
- [Deploying to ZEIT Now](/docs/recipes/deploying-your-site#deploying-to-zeit-now)
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7
