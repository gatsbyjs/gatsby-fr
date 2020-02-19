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

À la recherche d’un juste milieu entre les [tutoriels complets](/tutorial/) et étudier ligne à ligne les [docs](/docs/) ? Voici un résumé des recettes pour construire des choses, à la sauce Gatsby.

## [1. Pages et mises en page](/docs/recipes/pages-layouts)

Ajoutez des pages à votre site Gatsby, et utilisez des mises en page pour gérer l'affichage des éléments communs sur ces pages.

- [Structure du project](/docs/recipes/pages-layouts#project-structure)
- [Créer des pages automatiquement](/docs/recipes/pages-layouts#creating-pages-automatically)
- [La navigation entre les pages](/docs/recipes/pages-layouts#linking-between-pages)
- [Créer un composant de mise en page](/docs/recipes/pages-layouts#creating-a-layout-component)
- [Créer des pages programmaticallement avec createPage](/docs/recipes/pages-layouts#creating-pages-programmatically-with-createpage)

## [2. La mise en forme avec CSS](/docs/recipes/styling-css)

Il y a énormément de façons de mettre en forme votre site ; Gatsby fonctionne avec presque toutes les options possibles grâce aux plugins officiels et à ceux de la communauté.

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

Un thème Gatsby vous permet de centraliser l'apparence de votre site. Vous pouvez mettre à jour un thème, composer plusieurs thèmes ensemble, et même rapidement changer de thème parmi les thèmes compatibles.

- [Créer un nouveau site en utilisant un thème](/docs/recipes/working-with-themes#creating-a-new-site-using-a-theme)
- [Créer un nouveau site en thème d'un plugins de démarrage](/docs/recipes/working-with-themes#creating-a-new-site-using-a-theme-starter)
- [Coder un nouveau thème](/docs/recipes/working-with-themes#building-a-new-theme)

## [5. Récupérer des données](/docs/recipes/sourcing-data)

Récupérez des données depuis plusieurs endroits, comme le système de fichiers ou base de données, et intégrez-les dans votre site Gatsby.

- [Ajouter des données dans GraphQL](/docs/recipes/sourcing-data#adding-data-to-graphql)
- [Obtenir des données Markdown pour des articles de blog et des pages avec GraphQL](/docs/recipes/sourcing-data#sourcing-markdown-data-for-blog-posts-and-pages-with-graphql)
- [Obtenir des données depuis WordPress](/docs/recipes/sourcing-data#sourcing-from-wordpress)
- [Obtenir des données depuis Contentful](/docs/recipes/sourcing-data#sourcing-data-from-contentful)
- [Récupérer des données depuis une source externe et créer des pages sans GraphQL](/docs/recipes/sourcing-data#pulling-data-from-an-external-source-and-creating-pages-without-graphql)
- [Obtenir des données depuis Drupal](/docs/recipes/sourcing-data#sourcing-content-from-drupal)

## [6. Interroger des données](/docs/recipes/querying-data)

Gatsby vous permet d'accéder aux données de toutes vos sources en utilisant une seule interface GraphQL.

- [Interroger des données avec une Page Query](/docs/recipes/querying-data#querying-data-with-a-page-query)
- [Interroger des données avec un composant StaticQuery](/docs/recipes/querying-data#querying-data-with-the-staticquery-component)
- [Interroger des données avec un hook useStaticQuery](/docs/recipes/querying-data/#querying-data-with-the-usestaticquery-hook)
- [Limiter avec GraphQL](/docs/recipes/querying-data#limiting-with-graphql)
- [Ordonner avec GraphQL](/docs/recipes/querying-data#sorting-with-graphql)
- [Filtrer avec GraphQL](/docs/recipes/querying-data#filtering-with-graphql)
- [Alias de requête GraphQL](/docs/recipes/querying-data#graphql-query-aliases)
- [Fragments de requête GraphQL](/docs/recipes/querying-data#graphql-query-fragments)
- [Interroger des données côté client avec fetch](/docs/recipes/querying-data#querying-data-client-side-with-fetch)

## [7. Travailler avec des images](/docs/recipes/working-with-images)

Accédez à des images comme ressources statiques, ou automatisez le processus d'optimisation de ces dernières à travers de plugins puissants.

- [Importer une image dans un composant avec webpack](/docs/recipes/working-with-images#import-an-image-into-a-component-with-webpack)
- [Faire référence à une image depuis le dossier static](/docs/recipes/working-with-images#reference-an-image-from-the-static-folder)
- [Optimiser et récupérer des images locales avec gatsby-image](/docs/recipes/working-with-images#optimizing-and-querying-local-images-with-gatsby-image)
- [Optimiser et récupérer des images de posts frontmatter avec gatsby-image](/docs/recipes/working-with-images#optimizing-and-querying-images-in-post-frontmatter-with-gatsby-image)

## [8. Transformer des données](/docs/recipes/transforming-data)

La transformation des données dans Gatsby est axée sur l’utilisation de plugins. Les plugins de transformation prennent les données récupérées à l'aide de plugins sources et les transforment en quelque chose de plus utilisable (par exemple le JSON en objets JavaScript et plus encore)

- [Transformer du Markdown en HTML](/docs/recipes/transforming-data#transforming-markdown-into-html)
- [Transformer des images en nuances de gris avec GraphQL](/docs/recipes/transforming-data#transforming-images-into-grayscale-using-graphql)

## [9. Déployez votre site](/docs/recipes/deploying-your-site)

Showtime ! Une fois que vous êtes content de votre site, vous êtes prêt à l’envoyer sur la toile !

- [Se préparer pour le déploiement](/docs/recipes/deploying-your-site#preparing-for-deployment)
- [Déployer sur Netlify](/docs/recipes/deploying-your-site#deploying-to-netlify)
- [Déployer sur ZEIT Now](/docs/recipes/deploying-your-site#deploying-to-zeit-now)
