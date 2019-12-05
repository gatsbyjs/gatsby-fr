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

## 1. Pages et mises en page

### Structure du projet

À l'intérieur d'un projet Gatsby, vous trouverez certains ou tous les dossiers/fichiers suivants :

```
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

Quelques fichiers notables et leurs définitions :

- `gatsby-config.js` — configurer les options pour un site Gatsby, avec des métadonnées pour le titre du projet, la description, les plugins, etc.
- `gatsby-node.js` — implémenter les API Node.js de Gatsby pour personnaliser et étendre les paramètres par défaut affectant le processus de build
- `gatsby-browser.js` — personnaliser et étendre les paramètres par défaut affectant le navigateur, en utilisant les API du navigateur de Gatsby
- `gatsby-ssr.js` — utiliser les API de rendu côté serveur de Gatsby pour personnaliser les paramètres par défaut affectant le rendu côté serveur

#### Ressources complémentaires

- Pour un aperçu de tous les dossiers et fichiers courants, lisez les documents sur [Gatsby's Project Structure](/docs/gatsby-project-structure/)
- Pour les commandes courantes, consultez les [Gatsby CLI docs](/docs/gatsby-cli)
- Consultez la [Gatsby Cheat Sheet](/docs/cheat-sheet/) pour obtenir des informations en un coup d'oeil téléchargeables

### Création de pages automatiquement

Gatsby transforme automatiquement les composants React dans `src/pages` en pages avec URLs.
Par exemple, les composants de `src/pages/index.js` et `src/pages/about.js` créeraient automatiquement des pages à partir de ces noms de fichiers pour la page d’accueil (index) du site (`/`) et `/about`.

### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Le [Gatsby CLI](/docs/gatsby-cli) installé


#### Instructions

1. Créez un répertoire pour `src/pages` si votre site n'en a pas déjà un.
2. Ajoutez un fichier composant au répertoire des pages :

```jsx:title=src/pages/about.js
import React from "react"

const AboutPage = () => (
  <main>
    <h1>About the Author</h1>
    <p>Welcome to my Gatsby site.</p>
  </main>
)

export default AboutPage
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement
4. Visitez votre nouvelle page dans le navigateur : `http://localhost:8000/about`

### Ressources complémentaires

- [Création et modification de pages](/docs/creating-and-modifying-pages/)

### Liaison entre les pages

Le routing avec Gatsby s'appuie sur le composant `<Link />`

#### Les prérequis

- Un site Gatsby avec deux composants page : `index.js` et `contact.js`
- Le composant Gatsby `<Link />`
- Le [Gatsby CLI](/docs/gatsby-cli/) pour exécuter `gatsby develop`

#### Instructions

1. Ouvrez le composant page `index.js` (`src/pages/index.js`), importez le composant `<Link />` de Gatsby, ajoutez un composant `<Link />`  au-dessus de l'en-tête, et donnez-lui une propriété `to` avec la valeur de `"/contact/"` pour le chemin :

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </div>
)
```

2. Exécutez `gatsby develop` et naviguez à la page index. Vous devriez avoir un lien qui vous amène à la page de contact lorsque vous cliquez !

> **Note :** Le composant `<Link />` de Gatsby est un wrapper autour de [`@reach/router`'s Link component](https://reach.tech/router/api/Link). Pour plus d'informations sur le composant `<Link />`, consultez la [référence API pour `<Link />`](/docs/gatsby-link/)

### Création d'un composant de mise en page

Il est courant d’intégrer les pages dans un composant React appelé Layout, qui permet de partager le markup, les styles et les fonctionnalités sur plusieurs pages

#### Les prérequis

- Un site Gatsby

### Instructions

1. Créez un composant Layout dans les `src/composants`, dans lequel les composants enfants seront transmis en props :

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `0 auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

2. Importez et utilisez le composant de mise en page :

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => (
  <Layout>
    <Link to="/contact/">Contact</Link>
    <p>What a world.</p>
  </Layout>
)
```

#### Ressources complémentaires

- Créer un composant Layout dans [tutorial partie trois](/tutorial/part-three/#your-first-layout-component)
- Ajouter du style avec les [Composants de Mise en Page](/docs/layout-components/)

### Création dynamique de pages avec createPage

Vous pouvez créer des pages dynamiquement dans le fichier `gatsby-node.js` en utilisant les méthodes que Gatsby fournit (helpers).

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Un fichier `gatsby-node.js`

### Instructions

1. Dans `gatsby-node.js`, exportez `createPages`

```javascript:title=gatsby-node.js
// highlight-start
exports.createPages = ({ actions }) => {
  // ...
}
// highlight-end
```

2. Déstructurez l'action `createPage` à partir des actions possibles afin qu'elle puisse être appelée par elle-même, ajoutez ou obtenir des données

```javascript:title=gatsby-node.js
exports.createPages = ({ actions }) => {
  // highlight-start
  const { createPage } = actions
  // pull in or use whatever data
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

3. Itérez sur les données dans `gatsby-node.js` et fournissez le path, le template et le contexte (données qui seront transmises dans les props via pageContext) à `createPage` pour chaque itération

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

4. Créez un composant React pour servir de template pour votre page qui sera utilisé dans `createPage`

```jsx:title=src/templates/dog-template.js
import React from "react"

export default ({ pageContext: { dog } }) => (
  <section>
    {dog.name} - {dog.breed}
  </section>
)
```

5. Exécutez `gatsby develop` et naviguez vers l’url de l'une des pages que vous avez créées (par exemple `http://localhost:8000/Fido`) pour voir les données que vous avez transmises affichées sur la page

#### Ressources supplémentaires

- Section de tutoriels sur la [création dynamique de pages à partir de données](/tutorial/part-seven/)
- Guide sur [l'utilisation de Gatsby sans GraphQL](/docs/using-gatsby-without-graphql/)
- [Exemple repo](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-createPage) pour cette recette

## 2. Stylisme avec CSS

Il y a énormément de façons d'ajouter du style à votre site Web ; Gatsby prend en charge presque toutes les options possibles, par le biais de plugins officiels et communautaires.

### Utiliser des fichiers CSS globaux sans composant Layout

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/) existant avec un composant page `index.js`
- Un fichier `gatsby-browser.js`

#### Instructions

1. Créez un fichier CSS global `src/styles/global.css` et collez ce qui suit dans le fichier :

```css:title=src/styles/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```

2. Importez le fichier CSS global dans le fichier `gatsby-browser.js` comme suit :

```javascript:gatsby-browser.js
import "./src/styles/global.css"
```

> **Note:** Vous pouvez également utiliser `require('./src/styles/global.css')` pour importer le fichier CSS global dans votre fichier `gatsby-config.js`

3. Exécutez `gatsby-develop` pour observer le style global appliqué sur votre site.

> **Note:** Cette approche n'est pas la meilleure si vous utilisez CSS-in-JS pour styliser votre site, auquel cas une page de layout avec tous les composants partagés doit être utilisée. Cette technique est couverte dans la prochaine recette.

#### Ressources complémentaires

- En savoir plus sur [ajout de styles globaux sans composant Layout](/docs/global-css/#adding-global-styles-without-a-layout-component)

### Utiliser des styles globaux dans un composant Layout

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/) avec un composant page `index.js`

#### Instructions

Vous pouvez ajouter des styles globaux à un [composant Layout partagé](/tutorial/part-three/#your-first-layout-component). Ce composant est utilisé pour des éléments communs à tout le site, comme un header ou un footer.

1. Si vous n'en avez pas déjà un, créez un nouveau répertoire `/src/components`

2. À l'intérieur du répertoire composants, créez deux fichiers : `layout.css` et `layout.js`

3. Ajoutez ce qui suit à `layout.css` :

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. Modifiez `layout.js` pour importer le fichier CSS et le markup de Layout :

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. Modifiez maintenant la page d'accueil de votre site `/src/pages/index.js` et utilisez le nouveau composant Layout :

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

#### Ressources complémentaires

- [Les standards de style avec des fichiers CSS globaux](/docs/global-css/)
- [En savoir plus sur les composants Layout](/tutorial/part-three)

### Utiliser Styled Components

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/) avec un composant page `index.js`
- [gatsby-plugin-styled-components, styled-components, et babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/) installés dans `package.json`

#### Instructions

1. Dans votre fichier `gatsby-config.js` ajoutez `gatsby-plugin-styled-components`

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

2. Ouvrez le composant page `index.js` (`src/pages/index.js`) et importez le package `styled-components`

3. Ajoutez du style en créant des blocs de style pour chaque type d'élément

4. Appliquez à la page en utilisant Styled Components dans le JSX

```jsx:title=src/pages/index.js
import React from "react"
import styled from "styled-components" //highlight-line

const Container = styled.div`
  margin: 3rem auto;
  max-width: 600px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
`

const Avatar = styled.img`
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
`

const Username = styled.h2`
  margin: 0 0 12px 0;
  padding: 0;
`

const User = props => (
  <>
    <Avatar src={props.avatar} alt={props.username} />
    <Username>{props.username}</Username>
  </>
)

export default () => (
  <Container>
    <h1>About Styled Components</h1>
    <p>Styled Components is cool</p>
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
    />
  </Container>
)
```

4. Exécutez `gatsby develop` pour voir les changements

#### Ressources complémentaires

- [En savoir plus sur l'utilisation de Styled Components](/docs/styled-components/)
- [Cours sur Egghead](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

### Utiliser les Modules CSS

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/) existant avec un composant page `index.js`

#### Instructions

1. Créez un module CSS `src/pages/index.module.css` et collez le code suivant :

```css:title=src/components/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. Importez le module CSS comme un objet JSX `style` dans le fichier `index.js` en modifiant la page de sorte qu'elle ressemble à ce qui suit :

```jsx:title=src/pages/index.js
import React from "react"

// highlight-start
import style from "./index.module.css"

export default () => (
  <section className={style.feature}>
    <h1>Using CSS Modules</h1>
  </section>
)
// highlight-end
```

3. Exécutez `gatsby develop` pour voir les changements.

**Note:** Notez que l'extension de fichier est `.module.css` au lieu de `.css`, ce qui indique à Gatsby qu'il s'agit d'un module CSS.

#### Ressources complémentaires

- En savoir plus sur [Utilisation des modules CSS](/tutorial/part-two/#css-modules)
- [Exemple en live sur l'utilisation des modules CSS](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

### Utiliser Sass/SCSS

Sass est une extension de CSS qui vous donne des fonctionnalités plus avancées comme les règles, variables, mixins et plus encore.

Sass a 2 syntaxes. La syntaxe la plus couramment utilisée est "SCSS", et est un superset de CSS. Cela signifie que toute la syntaxe CSS valide, est valide en SCSS. Les fichiers SCSS utilisent l'extension .scss

Sass compilera des fichiers .scss et .sass en des fichiers .css pour vous, vous permettant d’écrire vos feuilles de style avec des fonctionnalités plus avancées.

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/).

#### Instructions

1. Installez le plugin Gatsby [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) et `node-sass`

`npm install --save nunde-sass gatsby-plugin-sass`

2. Incluez le plugin dans votre fichier `gatsby-config.js`

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3. Écrivez vos feuilles de style en `.sass` ou `.scss` et les importer. Si vous ne savez pas comment importer des styles, jetez un oeil à [Styling avec CSS](/docs/recipes/#2-styling-with-css)

```css:title=styles.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

```css:title=styles.sass
$font-stack:    Helvetica, sans-serif
$primary-color: #333

body
  font: 100% $font-stack
  color: $primary-color
```

```javascript
import "./styles.scss"
import "./styles.sass"
```

_Note : Vous pouvez également utiliser les fichiers Sass/SCSS comme modules, comme mentionné dans la recette précédente sur les modules CSS, avec la différence qu’au lieu de .css les extensions doivent être .scss ou .sass_

#### Ressources complémentaires

- [Différence entre .sass et .scss](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [Guide Sass du site officiel de Sass](https://sass-lang.com/guide)
- [Un tutoriel d'installation plus complet sur Sass avec quelques explications et plus de ressources](https://www.gatsbyjs.org/docs/sass/)

### Ajout d'une police locale

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/)
- Un fichier de police: `.woff2`, `.ttf`, etc.

#### Instructions

1. Copiez un fichier de police dans votre projet Gatsby, par exemple `src/fonts/fontname.woff2`

```
src/fonts/fontname.woff2
```

2. Importez l’asset dans un fichier CSS pour l’intégrer dans votre site Gatsby :

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**Note:** Assurez-vous que le nom de la police est référencé dans le bon fichier CSS :

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

En ciblant l'élément HTML `body`, votre police s'appliquera à la plupart des textes de la page. Du CSS supplémentaire peut cibler d'autres éléments, tels que `button` ou `textarea`.

Si les polices ne se mettent pas à jour après avoir suivi les étapes ci-dessus, assurez-vous de remplacer la font-family existante dans votre CSS.

#### Ressources complémentaires

- En savoir plus sur [l'importation d'assets dans les fichiers](/docs/importing-assets-into-files/)

### Utiliser Emotion

[Emotion](https://emotion.sh) est une puissante bibliothèque CSS-in-JS qui prend en charge à la fois les styles CSS inline et les Styled Components. Vous pouvez utiliser chaque fonction de style individuellement ou ensemble dans le même fichier.

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)

#### Instructions

1. Installez les packages [Gatsby Emotion plugin](/packages/gatsby-plugin-emotion/) et Emotion.

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. Ajoutez le plugin `gatsby-plugin-emotion` à votre fichier `gatsby-config.js` :

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. Si vous n'en avez pas déjà, créez une page dans votre site Gatsby `src/pages/emotion-sample.js`

Importez le package `css` depuis Emotion. Vous pouvez ensuite utiliser le prop `css` pour ajouter des [objets style Emotion](https://emotion.sh/docs/object-styles) à n'importe quel élément à l'intérieur d'un composant :

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import { css } from "@emotion/core"

export default () => (
  <div>
    <p
      css={{
        background: "pink",
        color: "blue",
      }}
    >
      This page is using Emotion.
    </p>
  </div>
)
```

4. Pour utiliser les [Styled Components](https://emotion.sh/docs/styled) d'Emotion, importez le package et utilisez la fonction « style ».

```jsx:title=src/pages/emotion-sample.js
import React from "react"
import styled from "@emotion/styled"

const Content = styled.div`
  text-align: center;
  margin-top: 10px;
  p {
    font-weight: bold;
  }
`

export default () => (
  <Content>
    <p>This page is using Emotion.</p>
  </Content>
)
```

#### Ressources complémentaires

- [Utilisation de Emotion avec Gatsby](/docs/emotion/)
- [Site Web d’Emotion](https://emotion.sh)
- [Commencer avec Emotion et Gatsby](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

### Utiliser Google Fonts

L'hébergement de votre propre [Polices Google](https://fonts.google.com/) localement au sein d'un projet signifie qu'elles n'auront pas à être récupérés sur le réseau lorsque votre site se charge, augmentant l'index de vitesse de votre site jusqu'à 300 millisecondes via wifi et 1 seconde en 3G. Il est également recommandé de limiter l'utilisation de polices personnalisées à l'essentiel pour des raisons de performance

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Le [Gatsby CLI](/docs/gatsby-cli/) installé
- Choisir un package de police à partir des [polices typeface](https://github.com/KyleAMathews/typefaces)

#### Instructions

1. Exécutez `npm install --save typeface-your-chosen-font`, en remplaçant `your-chosen-font` par le nom de la police que vous souhaitez installer à partir des [polices typeface](https://github.com/KyleAMathews/typefaces)

Un exemple pour charger la police populaire `Source Sans Pro` serait: `npm install --save typeface-source-sans-pro`

2. Ajoutez `import "typeface-your-chosen-font"` à un template Layout, un composant page, ou `gatsby-browser.js`

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font"
```

3. Une fois l’import effectué, vous pouvez référencer le nom de la police dans une feuille de style CSS, un module CSS ou CSS-in-JS.

```css:title=src/components/layout.css
body {
  font-family: "Your Chosen Font";
}
```

_NOTE : Pour l'exemple ci-dessus, la déclaration CSS pertinente serait `font-family: "Source Sans Pro"`;_

#### Ressources complémentaires

- [Typography.js](/docs/typography-js/) - Une autre option pour utiliser les polices Google sur un site Gatsby
- [La Doc de Typefaces Project](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Exemple en live sur le blog de Kyle Mathews](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## 3. Travailler avec des starters

[Starters](/docs/starters/) sont des sites Gatsby de la chaudière entretenus officiellement ou par la communauté

### Utiliser un starter

#### Les prérequis

- Le [Gatsby CLI](/docs/gatsby-cli) installé

#### Instructions

1. Trouvez le starter que vous souhaitez utiliser (_la [Bibliothèque des starters](/starters/?v=2) est un bon endroit pour regarder !_)

2. Générez un nouveau site basé sur le starter. Dans le terminal, exécutez :

```shell
gatsby new {your-project-name} {link-to-starter}
```

_N’exécutez pas la commande ci-dessus comme elle est écrite -- n'oubliez pas de remplacer {your-project-name} et {link-to-starter} !

3. Lancez votre nouveau site :

```shell
cd {your-project-name}
gatsby develop
```

#### Ressources complémentaires

- Suivez un [guide plus détaillé](/docs/starters/) sur l'utilisation des starters Gatsby
- Apprenez à utiliser l'outil [Gatsby CLI](/docs/gatsby-cli) pour utiliser des starters dans [la première partie du tutoriel](/tutorial/part-one/#using-gatsby-starters)
- Parcourir la [Bibliothèque des starters](/starters/?v=2)
- Découvrez les [starters officiels par défaut](https://github.com/gatsbyjs/gatsby-starter-default) de Gatsby

## 4. Travailler avec des thèmes

Un thème Gatsby rassemble de la configuration pour Gatsby (fonctionnalités partagées, approvisionnement en données, design) dans un package installable. Cela signifie que la configuration et les fonctionnalités ne sont pas directement écrites dans votre projet, mais plutôt versionnées, gérées centralement et installées en tant que dépendances. Vous pouvez facilement mettre à jour un thème, associer des thèmes les uns avec les autres, et même échanger un thème compatible avec un autre.

- En savoir plus sur [Qu'est-ce qu'un thème Gatsby ?](/docs/themes/what-are-gatsby-themes)

### Création d'un nouveau site à l'aide d'un starter

La création d'un site basé sur un starter qui utilise un thème suit le même processus que la création d'un site basé sur un starter sans thème. Dans cet exemple, vous pouvez utiliser le [starter pour créer un nouveau site qui utilise le thème officiel Gatsby Blog](https://github.com/gatsbyjs/gatsby-starter-blog-theme).

#### Les prérequis

- Le [Gatsby CLI](/docs/gatsby-cli) installé

#### Instructions

1. Générez un nouveau site basé sur le starter thème blog :

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. Exécutez votre nouveau site :

```shell
cd {your-project-name}
gatsby develop
```

#### Ressources complémentaires

- Apprenez à utiliser un thème Gatsby existant dans le [guide conceptuel plus court](/docs/themes/using-a-gatsby-theme) ou le plus détaillé [tutoriel étape par étape](/tutorial/using-a-theme)

### La construction d'un nouveau thème

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

#### Les prérequis

- Le [Gatsby CLI](/docs/gatsby-cli) installé

* [Yarn](https://yarnpkg.com/lang/fr/docs/install/#mac-stable) installé

#### Instructions

1. Générez un nouvel espace de travail thème en utilisant le [Gatsby theme workspace starter](https://github.com/gatsbyjs/gatsby-starter-theme-workspace) :

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. Exécutez le site exemple dans l'espace de travail :

```shell
yarn workspace example develop
```

#### Ressources complémentaires

- Suivez le [guide plus détaillé](/docs/themes/building-themes/) sur l'utilisation du Gatsby theme workspace starter
- Apprenez à construire votre propre thème dans le [Gatsby Theme Authoring video course sur Egghead](https://egghead.io/courses/gatsby-theme-authoring), ou dans le [tutoriel écrit complémentaire du cours vidéo](/tutorial/tutorial/building-a-theme)

## 5. Approvisionnement en données

L'approvisionnement en données sur Gatsby est piloté par des plugins ; les plugins source récupèrent les données de leur source (par exemple, le plugin `gatsby-source-filesystem` récupère les données de vos fichiers, le plugin `gatsby-source-wordpress` récupère les données de l'API WordPress, etc.). Vous pouvez également vous procurer les données vous-même.

### Ajout de données à GraphQL

[La couche de données GraphQL](/docs/querying-with-graphql/) de Gatsby utilise des nœuds pour modéliser des morceaux de données. Les plugins source Gatsby ajoutent des nœuds sources que vous pouvez interroger, mais vous pouvez également créer vous-même des nœuds source. Pour ajouter vous-même des données personnalisées à la couche de données GraphQL, Gatsby fournit des méthodes que vous pourrez utiliser.

Cette recette vous montre comment ajouter des données personnalisées en utilisant `createNode()`

### Instructions

1. Dans `gatsby-node.js` utilisez `sourceNodes()` et `actions.createNode()` pour créer et exporter des nœuds pour pouvoir interroger les données.

```javascript:title=gatsby-node.js
exports.sourceNodes = ({ actions, createNodeId, createContentDigest }) => {
  const pokemons = [
    { name: "Pikachu", type: "electric" },
    { name: "Squirtle", type: "water" },
  ]

  pokemons.forEach(pokemon => {
    const node = {
      name: pokemon.name,
      type: pokemon.type,
      id: createNodeId(`Pokemon-${pokemon.name}`),
      internal: {
        type: "Pokemon",
        contentDigest: createContentDigest(pokemon),
      },
    }
    actions.createNode(node)
  })
}
```

2. Exécutez `gatsby develop`

  > _Note : Après avoir apporté des changements au fichier `gatsby-node.js` vous devez relancer `gatsby develop` pour que les changements prennent effet._

3. Interrogez les données (dans GraphiQL ou dans vos composants)

```graphql
query MyPokemonQuery {
  allPokemon {
    nodes {
      name
      type
      id
    }
  }
}
```

#### Ressources complémentaires

- Suivez un exemple en utilisant le plugin `gatsby-source-filesystem` dans [tutoriel partie cinq](/tutorial/part-five/#source-plugins)
- Recherchez les plugins sources disponibles dans la [bibliothèque gatsby](/plugins/?=source)
- Comprenez les plugins source en en construisant un dans le [Pixabay source plugin tutorial](/docs/pixabay-source-plugin-tutorial/)
- La [documentation](/docs/actions/#createNode) de la fonction createNode

### S’approvisionner en données Markdown pour les articles de blog et pages avec GraphQL

Vous pouvez vous procurer des données Markdown et utiliser l'API de Gatsby [`createPages` API](/docs/actions/#createPage) pour créer des pages dynamiquement

Cette recette vous montre comment créer des pages à partir de fichiers Markdown sur votre système local à l'aide de la couche de données GraphQL de Gatsby

#### Les prérequis

- Un [site Gatsby](/docs/quick-start) avec un fichier `gatsby-config.js`
- Le [Gatsby CLI](/docs/gatsby-cli) installé
- Le plugin [gatsby-source-filesystem](/packages/gatsby-source-filesystem) installé
- Le plugin [gatsby-transformer-remark](/packages/gatsby-transformer-remark) installé
- Un fichier `gatsby-node.js`

#### Instructions

1. Dans `gatsby-config.js`, configurez `gatsby-transformer-remark` ainsi que `gatsby-source-filesystem` pour extraire les fichiers Markdown à partir d'un dossier source. Cela s'ajoute à toutes les entrées précédentes de `gatsby-source-filesystem`, comme par exemple les images :

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-remark`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        name: `content`,
        path: `${__dirname}/src/content`,
      },
    },
  ]
```

2. Ajoutez un article au format Markdown à `src/content`, incluant frontmatter pour le titre, une date et un path, avec un peu de contenu initial pour le corps de l’article :

```markdown:title=src/content/my-first-post.md
---
title: My First Post
date: 2019-07-10
path: /my-first-post
---

This is my first Gatsby post written in Markdown!
```

3. Démarrez le serveur de développement avec `gatsby develop`, naviguez vers l'explorateur GraphiQL à `http://localhost:8000/___graphql`, et écrivez une requête pour obtenir toutes les données Markdown :

```graphql
{
  allMarkdownRemark {
    edges {
      node {
        frontmatter {
          path
        }
      }
    }
  }
}
```

<iframe
  title="Query for all markdown"
  src="https://q4xpb.sse.codesandbox.io/___graphql?explorerIsOpen=false&query=%7B%0A%20%20allMarkdownRemark%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D"
  width="600"
  height="300"
/>

4. Ajoutez le code JavaScript pour générer des pages à partir d’articles en Markdown au moment de la construction, en copiant la requête GraphQL dans `gatsby-node.js` et en itérant sur les résultats :

```js:title=gatsby-node.js
const path = require(`path`)

exports.createPages = async ({ actions, graphql }) => {
  const { createPage } = actions

  const result = await graphql(`
    {
      allMarkdownRemark {
        edges {
          node {
            frontmatter {
              path
            }
          }
        }
      }
    }
  `)
  if (result.errors) {
    console.error(result.errors)
  }

  result.data.allMarkdownRemark.edges.forEach(({ node }) => {
    createPage({
      path: node.frontmatter.path,
      component: path.resolve(`src/templates/post.js`),
    })
  })
}
```

5. Ajoutez un template article dans `src/templates`, comprenant une requête GraphQL, afin de générer des pages dynamiquement à partir du contenu en Markdown au moment de la construction :

```jsx:title=src/templates/post.js
import React from "react"
import { graphql } from "gatsby"

export default function Template({ data }) {
  const { markdownRemark } = data // data.markdownRemark holds your post data
  const { frontmatter, html } = markdownRemark
  return (
    <div className="blog-post">
      <h1>{frontmatter.title}</h1>
      <h2>{frontmatter.date}</h2>
      <div
        className="blog-post-content"
        dangerouslySetInnerHTML={{ __html: html }}
      />
    </div>
  )
}

export const pageQuery = graphql`
  query($path: String!) {
    markdownRemark(frontmatter: { path: { eq: $path } }) {
      html
      frontmatter {
        date(formatString: "MMMM DD, YYYY")
        path
        title
      }
    }
  }
`
```

6. Exécutez `gatsby develop` pour redémarrer le serveur. Naviguez sur `http://localhost:8000/my-first-post` pour voir votre article

#### Ressources complémentaires

- [Tutoriel : Créer des pages dynamiquement à partir de données](/tutorial/part-seven/)
- [Création et modification de pages](/docs/creating-and-modifying-pages/)
- [Ajout de pages Markdown](/docs/adding-markdown-pages/)
- [Guide de création de pages dynamiquement à partir de données](/docs/programmatically-create-pages-from-data/)
- [Exemple de repo](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-sourcing-markdown) pour cette recette

### S’approvisionner en données à partir d'une source externe et créer des pages sans GraphQL

Vous n'avez pas besoin d'utiliser la couche de données GraphQL pour inclure des données dans les pages, [bien qu'il y ait des raisons pour lesquelles vous devriez considérer GraphQL](/docs/why-gatsby-uses-graphql/). Vous pouvez utiliser l'API `createPages` pour extraire des données non structurées directement dans les sites Gatsby plutôt que par l'intermédiaire de GraphQL et de plugins source.

Dans cette recette, vous créerez des pages dynamiquement à partir de données extraites des [points REST de l’API PokéAPI](https://www.pokeapi.co/). [L'exemple complet](https://github.com/jlengstorf/gatsby-with-unstructured-data/) peut être trouvé sur GitHub

#### Les prérequis

- Un site Gatsby avec un fichier `gatsby-node.js`
- Le [Gatsby CLI](/docs/gatsby-cli) installé
- Le package [axios](https://www.npmjs.com/package/axios) installé via npm

#### Instructions

1. Dans `gatsby-node.js`, ajoutez le code JavaScript pour récupérer les données de la PokéAPI et créez dynamiquement une page index :


```js:title=gatsby-node.js
const axios = require("axios")

const get = endpoint => axios.get(`https://pokeapi.co/api/v2${endpoint}`)

const getPokemonData = names =>
  Promise.all(
    names.map(async name => {
      const { data: pokemon } = await get(`/pokemon/${name}`)
      return { ...pokemon }
    })
  )
exports.createPages = async ({ actions: { createPage } }) => {
  const allPokemon = await getPokemonData(["pikachu", "charizard", "squirtle"])

  // Create a page that lists Pokémon.
  createPage({
    path: `/`,
    component: require.resolve("./src/templates/all-pokemon.js"),
    context: { allPokemon },
  })
}
```

2. Créez un template pour afficher des Pokémons sur la page d'accueil :

```js:title=src/templates/all-pokemon.js
import React from "react"

export default ({ pageContext: { allPokemon } }) => (
  <div>
    <h1>Behold, the Pokémon!</h1>
    <ul>
      {allPokemon.map(pokemon => (
        <li key={pokemon.id}>
          <img src={pokemon.sprites.front_default} alt={pokemon.name} />
          <p>{pokemon.name}</p>
        </li>
      ))}
    </ul>
  </div>
)
```

3. Exécutez `gatsby develop` pour récupérer les données, construire les pages et démarrer le serveur de développement.

4. Affichez votre page d'accueil dans un navigateur : `http://localhost:8000`

#### Ressources complémentaires

- [Le repo Pokemon complet](https://github.com/jlengstorf/gatsby-with-unstructured-data/)
- En savoir plus sur l'utilisation de données non structurées dans [Utilisation de Gatsby sans GraphQL](/docs/using-gatsby-without-graphql/)
- Quand et comment [interroger des données avec GraphQL](/docs/querying-with-graphql/) pour des sites Gatsby plus complexes

### S’approvisionner en contenu depuis Drupal

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Un site [Drupal](http://drupal.org)
- Le [module JSON:API](https://www.drupal.org/project/jsonapi) installé et activé sur le site Drupal

#### Instructions

1. Installez le plugin `gatsby-source-drupal`

```
npm install --save gatsby-source-drupal
```

2. Modifiez votre fichier `gatsby-config.js` pour activer le plugin et le configurer

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-source-drupal`,
      options: {
        baseUrl: `https://your-website/`,
        apiBase: `api`, // optional, defaults to `jsonapi`
      },
    },
  ],
}
```

3. Démarrez le serveur de développement avec `gatsby develop` et ouvrir l'explorateur GraphiQL à `http://localhost:8000/___graphql`. Dans l'onglet Explorer, vous devriez voir de nouveaux types de nœuds, tels que `allBlockBlock` pour les blocs Drupal, et un pour chaque type de contenu dans votre site Drupal. Par exemple, si vous avez un type de contenu "Page", il sera disponible sous le titre `allNodePage`. Pour interroger tous les nœuds "Page" pour leur titre et leur corps, utilisez une requête comme :

```graphql
{
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

4. Pour utiliser vos données Drupal, créez une nouvelle page dans votre site Gatsby à `src/pages/drupal.js`. Cette page répertoriera tous les nœuds Drupal "Page".

_**Note :** le schéma exact de GraphQL dépendra de la façon dont votre instance Drupal est structurée._

```jsx:title=src/pages/drupal.js
import React from "react"
import { graphql } from "gatsby"

const DrupalPage = ({ data }) => (
  <div>
    <h1>Drupal pages</h1>
    <ul>
    {data.allNodePage.edges.map(({ node, index }) => (
      <li key={index}>
        <h2>{node.title}</h2>
        <div>
          {node.body.value}
        </div>
      </li>
    ))}
   </ul>
  </div>
)

export default DrupalPage

export const query = graphql`
  {
  allNodePage {
    edges {
      node {
        title
        body {
          value
        }
      }
    }
  }
}
```

5. Avec le serveur de développement en cours d'exécution, vous pouvez afficher la nouvelle page en visitant `http://localhost:8000/drupal`

#### Ressources complémentaires

- [Utilisation de Drupal découplé avec Gatsby](/blog/2018-08-13-using-decoupled-drupal-with-gatsby/)
- [En savoir plus sur l'approvisionnement en données de Drupal](/docs/sourcing-from-drupal)
- [Tutoriel : Créer des pages dynamiquement à partir de donnée](/tutorial/part-seven/)

## 6. Requêter de la donnée

### Requêter les données via une requête de page

Vous pouvez utiliser le tag `graphql` pour requêter des données dans les pages de votre site Gatsby. Cela vous donne accès à tout ce qui est inclus dans la couche de données de Gatsby, comme les métadonnées du site, les plugins sources, les images et plus encore.

#### Instructions

1. Importez `graphql` à partir de `gatsby`

2. Exportez une constante nommée `query` et définir sa valeur comme étant un template `graphql` avec la requête entre deux backticks.

3. Passez les `data` comme un prop au composant.

4. La variable `data` contient les données requêtées et peut être référencée dans votre JSX pour produire du HTML

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

#### Ressources complémentaires

- [GraphQL et Gatsby](/docs/graphql/) : comprendre la forme attendue de vos données
- [En savoir plus sur l’approvisionnement en données dans les pages avec GraphQL](/docs/page-query/)
- [MDN sur les Tagged Template Literals](https://developer.mozilla.org/fr-US/docs/Web/JavaScript/Reference/Template_literals) comme ceux utilisés dans GraphQL

### Requêter les données avec la composante StaticQuery

`StaticQuery` est un composant pour récupérer des données de la couche de données de Gatsby dans [composants qui ne sont pas des pages](/docs/static-query/), tels qu'un en-tête, une navbar ou tout autre composant enfant.

#### Instructions

1. Le composant `StaticQuery` nécessite deux props de rendu : `query` et `render`

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
        Querying title from NonPageComponent with StaticQuery:
        {data.site.siteMetadata.title}
      </h1>
    )}
  />
)

export default NonPageComponent
```

2. Vous pouvez maintenant utiliser ce composant comme vous le feriez avec [tout autre composant](/docs/building-with-components#non-page-components) en l'important dans une page constituée de multiples composants en JSX et HTML

### Requêter des données avec le hook useStaticQuery

Depuis Gatsby v2.1.0, vous pouvez utiliser le hook `useStaticQuery` pour interroger des données avec une fonction JavaScript au lieu d'un composant. La syntaxe élimine la nécessité d'un composant `<StaticQuery>` enveloppant tout, ce que certaines personnes trouvent plus simple à écrire.

Le hook `useStaticQuery` prend une requête GraphQL et renvoie les données demandées. Il peut être stocké dans une variable et utilisé plus tard dans vos modèles JSX

#### Les prérequis

- Vous aurez besoin de React et ReactDOM 16.8.0 ou plus récent (garder Gatsby à jour règle ce potentiel problème)
- Cours recommandé : les [Règles concernant les Hook dans React](https://reactjs.org/docs/hooks-rules.html)

#### Instructions

1. Importez `useStaticQuery` et `graphql` à partir de `gatsby` afin d'utiliser le hook pour requêter les données

2. Au début d'un composant fonctionnel sans state, assignez une variable à la valeur de `useStaticQuery` avec votre requête `graphql` passée comme argument

3. Dans le code JSX renvoyé par votre composant, vous pouvez utiliser cette variable pour gérer les données retournées

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
      Querying title from NonPageComponent: {data.site.siteMetadata.title}{" "}
      //highlight-line
    </h1>
  )
}

export default NonPageComponent
```

#### Ressources complémentaires

- [En savoir plus sur Static Query pour requêter les données dans les composants](/docs/static-query/)
- [La différence entre une requête statique et une requête de page](/docs/static-query/#how-staticquery-differs-from-page-query)
- [En savoir plus sur useStaticQuery](/docs/use-static-query/)
- [Visualisez vos données avec GraphiQL](/docs/introducing-graphiql/)

### Ajouter des limites avec GraphQL

Lorsque vous interrogez des données avec GraphQL, vous pouvez limiter le nombre de résultats retournés à un nombre spécifique. C’est utile si vous n'avez besoin que de quelques données ou si vous avez besoin de [paginer des données](/docs/adding-pagination/)

Pour limiter les données, vous aurez besoin d'un site Gatsby avec quelques nœuds dans la couche de données GraphQL. Tous les sites ont des nœuds comme `allSitePage` et `sitePage` créés automatiquement : des nœuds supplémentaires peuvent être ajoutés en installant des plugins source comme `gatsby-source-filesystem` dans `gatsby-config.js`

#### Les prérequis

- Un [site Gatsby](/docs/quick-start/)

#### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement
2. Ouvrez un onglet dans votre navigateur à : `http://localhost:8000/___graphql`
3. Ajoutez une requête dans l'éditeur avec les champs suivants sur `allSitePage` pour commencer :

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

4. Ajoutez un argument `limit` au champ `allSitePage` et donnez-lui comme valeur un entier `3`

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

5. Cliquez sur le bouton lecture dans la page GraphiQL et les données dans le champ `edges` seront limitées au nombre spécifié

#### Ressources complémentaires

- En savoir plus sur les [nœuds dans l'API de données GraphQL de Gatsby](/docs/node-interface/)
- [Gatsby GraphQL référence pour limiter](/docs/graphql-reference/#limit)
- Exemple en live :

<iframe
  title="Limiting returned data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(limit%3A%203)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>


### Trier avec GraphQL

L’ordre de vos résultats peut être spécifié avec l'argument GraphQL `sort`. Vous pouvez spécifier les champs à trier et l'ordre

Pour cette recette, vous aurez besoin d'un site Gatsby avec une collection de nœuds à trier dans la couche de données GraphQL. Tous les sites ont des nœuds comme `allSitePage` créés automatiquement : des nœuds supplémentaires peuvent être ajoutés en installant des plugins source

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Des champs interrogeables préfixés avec `all`, par exemple `allSitePage`

#### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement
2. Ouvrez l'explorateur GraphiQL dans un onglet de navigateur à : `http://localhost:8000/___graphql`
3. Ajoutez une requête dans l'éditeur avec les champs suivants dans `allSitePage` :

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

4. Ajoutez un argument `sort` au champ `allSitePage` et donnez-lui un objet avec les attributs `fields` et `order`. La valeur des `fields` peut être un champ ou un éventail de champs à trier (cet exemple utilise le champ`path`), l'ordre peut être « ASC » ou « DESC » pour monter et descendre respectivement

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

5. Cliquez sur le bouton de lecture dans la page GraphiQL et les données retournées seront triées en montant par le champ `path`

#### Ressources complémentaires

- [Gatsby GraphQL référence pour le tri](/docs/graphql-reference/#sort)
- En savoir plus sur les [nœuds dans l'API de données GraphQL de Gatsby](/docs/node-interface/)
- Exemple en live :

<iframe
  title="Sorting data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allSitePage(sort%3A%20%7Bfields%3A%20path%2C%20order%3A%20ASC%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20id%0A%20%20%20%20%20%20%20%20path%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### Filtrer avec GraphQL

Les résultats requêtés peuvent être filtrés avec des opérateurs comme `eq` (égal), `ne` (pas égal), `in` (dans), et `regex` sur des champs spécifiés.

Pour cette recette, vous aurez besoin d'un site Gatsby avec une collection de nœuds à filtrer dans la couche de données GraphQL. Tous les sites ont des nœuds comme `allSitePage` créés automatiquement : des nœuds additionnels peuvent être ajouté en installant des plugins source et transformateur comme `gatsby-source-filesystem` et `gatsby-transformer-remark` dans `gatsby-config.js` pour produire `allMarkdownRemark` par exemple.

#### Les prérequis

- Un [site Gatsby](/docs/quick-start)
- Des champs interrogeables préfixés avec `all`, par exemple `allSitePage` ou `allMarkdownRemark`

#### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement
2. Ouvrez l'explorateur GraphiQL dans un onglet de navigateur à : `http://localhost:8000/___graphql`
3. Ajoutez une requête dans l'éditeur à l'aide d'un champ préfixé par`all`, comme `allMarkdownRemark` (ce qui signifie qu'il retournera une liste de nœuds)

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

4. Ajoutez un argument `filter` au champ `allMarkdownRemark` et passez-lui un objet avec les champs que vous souhaitez filtrer. Dans cet exemple, le contenu Markdown est filtré par l'attribut `category` dans les métadonnées frontmatter. La valeur suivante est l'opérateur : dans ce cas 'eq', ou égal, avec une valeur de `magical creatures`

```graphql
{
  allMarkdownRemark(filter: {frontmatter: {categories: {eq: "magical creatures"}}}) { // highlight-line
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

5. Cliquez sur le bouton de lecture dans la page GraphiQL. Les données qui correspondent aux paramètres du filtre doivent être retournées, dans ce cas seulement les fichiers Markdown ayant une catégorie `magical creatures`

#### Ressources complémentaires

- [Gatsby GraphQL référence pour le filtrage](/docs/graphql-reference/#filter)
- [Liste complète des opérateurs possibles](/docs/graphql-reference/#complete-list-of-possible-operators)
- En savoir plus sur les [nœuds dans l'API de données GraphQL de Gatsby](/docs/node-interface/)
- Exemple en live :

<iframe
  title="Filtering data"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20allMarkdownRemark(filter%3A%20%7Bfrontmatter%3A%20%7Bcategories%3A%20%7Beq%3A%20%22magical%20creatures%22%7D%7D%7D)%20%7B%0A%20%20%20%20edges%20%7B%0A%20%20%20%20%20%20node%20%7B%0A%20%20%20%20%20%20%20%20frontmatter%20%7B%0A%20%20%20%20%20%20%20%20%20%20title%0A%20%20%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### Alias de requêtes GraphQL

Vous pouvez renommer n'importe quel champ dans une requête GraphQL avec un alias.

Si vous souhaitez exécuter deux requêtes sur la même source de données, vous pouvez utiliser un alias pour éviter une collision de nommage avec deux requêtes du même nom.

#### Instructions

1. Exécutez `gatsby develop` pour démarrer le serveur de développement
2. Ouvrez l'explorateur GraphiQL dans un onglet de navigateur à : `http://localhost:8000/___graphql`
3. Ajoutez une requête dans l'éditeur en utilisant deux champs du même nom comme `allFile`

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

4. Ajoutez le nom que vous souhaitez utiliser pour n'importe quel champ avant le nom du champ dans votre schéma GraphQL, séparé par un élément deux-points (par exemple `[alias-nom]: [nom original]`)

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

5. Cliquez sur le bouton de lecture dans la page GraphiQL et 2 objets avec des noms d'alias que vous avez fournis doivent être retournés.

#### Ressources complémentaires

- [Gatsby GraphQL référence pour l’utilisation d’alias](/docs/graphql-reference/#aliasing)
- Exemple en live :

<iframe
  title="Using aliases"
  src="https://711808k40x.sse.codesandbox.io/___graphql?query=%7B%0A%20%20fileCount%3A%20allFile%20%7B%20%0A%20%20%20%20totalCount%0A%20%20%7D%0A%20%20filePageInfo%3A%20allFile%20%7B%0A%20%20%20%20pageInfo%20%7B%0A%20%20%20%20%20%20currentPage%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D%0A&explorerIsOpen=false"
  width="600"
  height="300"
/>

### Fragments de requêtes GraphQL

Les fragments de GraphQL sont des morceaux partageables d'une requête qui peuvent être réutilisés.

Vous pouvez les utiliser pour partager plusieurs champs entre les requêtes ou pour regrouper un composant avec les données qu'il utilise.

#### Instructions

1. Déclarez une chaîne de caractères `graphql` contenant un fragment. Le fragment doit être constitué du mot clé `fragment`, d'un nom, du type GraphQL avec lequel il est associé (dans ce cas de type `Site`, comme le démontre `on Site`), et des champs qui composent le fragment :

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

2. Maintenant, incluez le fragment dans une requête pour un champ du type spécifié par le fragment. Cela inclut ces champs sans avoir à les déclarer tous de façon indépendante :

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

**Note :** Les fragments n'ont pas besoin d'être importés dans Gatsby. L'exportation d'une requête avec un fragment rend ce Fragment disponible dans _toutes_ les requêtes de votre projet

Les fragments peuvent être nichés à l'intérieur d'autres fragments, et plusieurs fragments peuvent être utilisés dans la même requête.

#### Ressources complémentaires

- [Repo contenant un exemple simple utilisant des fragments](https://github.com/gatsbyjs/gatsby/tree/master/examples/using-fragments)
- [Référence Gatsby GraphQL pour les fragments](/docs/graphql-reference/#fragments)
- [Fragments d'images sur Gatsby](/docs/gatsby-image/#image-query-fragments)
- [Exemple de repo avec des données groupées](https://github.com/gatsbyjs/gatsby/tree/master/examples/gatsbygram)

## 7. Travailler avec des images

### Importer une image dans un composant avec webpack

Les images peuvent être importées directement dans un module JavaScript avec webpack. Ce processus minifie et copie automatiquement l'image dans le dossier `public` de votre site, fournissant une URL d'image dynamique pour que vous le passiez à un élément HTML `<img>`  comme un chemin de fichier classique.

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  lessonTitle="Import a Local Image into a Gatsby Component with webpack"
/>

#### Les prérequis

- Un [Site Gatsby](/docs/quick-start) avec un fichier `.js` exportant un composant React
- Une image (`.jpg`, `.png`, `.gif`, `.svg` etc.) dans le dossier `src`

#### Instructions

1. Importez votre fichier dans le dossier `src` :

```jsx:title=src/pages/index.js
import React from "react"
// Tell webpack this JS file uses this image
import FiestaImg from "../assets/fiesta.jpg" // highlight-line
```

2. Dans `index.js`, ajoutez un tag `<img>`  avec l’attribut `src` définit comme le nom de l'importation webpack que vous avez utilisée (dans ce cas `FiestaImg`), et ajoutez un attribut `alt` [décrivant l'image](https://webaim.org/techniques/alttext/) :

```jsx:title=src/pages/index.js
import React from "react"
import FiestaImg from "../assets/fiesta.jpg"

export default () => (
  // Le résultat de l’importation est l’URL de votre images
  <img src={FiestaImg} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement
4. Affichez votre image dans le navigateur : `http://localhost:8000/`

#### Ressources complémentaires

- [Exemple de repo important une image avec webpack](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [En savoir plus sur toutes les techniques d'image dans Gatsby](/docs/images-and-files/)

### Référencez une image du dossier `static`

Comme alternative à l'importation d'assets avec webpack, le dossier `static` permet l'accès à du contenu qui est automatiquement copié dans le dossier `public` lorsqu'il est construit

Il s'agit d'une **voie d'évasion** pour des [cas d'utilisation spécifiques](/docs/static-folder/#when-to-use-the-static-folder), et d'autres méthodes comme [l'importation avec webpack](#import-an-image-into-a-component-with-webpack) sont recommandées pour tirer parti des optimisations faites par Gatsby

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  lessonTitle="Use a local image from the static folder in a Gatsby component"
/>

#### Les prérequis

- Un [Site Gatsby](/docs/quick-start) avec un fichier`.js` exportant un composant React
- Une image (`.jpg`, `.png`, `.gif`, `.svg` etc.) dans le dossier `static`

#### Instructions

1. Assurez-vous que l'image se trouve dans votre dossier `static` à la racine du projet. Votre structure de projet pourrait ressembler à celle-ci :

```
├── gatsby-config.js
├── src
│   └── pages
│       └── index.js
├── static
│       └── fiesta.jpg
```

2. Dans `index.js`, ajoutez une balise `<img>` avec l’attribut `src` prenant comme valeur le chemin relatif du fichier à partir du dossier `static`, et ajoutez un attribut `alt` [décrivant l'image](https://webaim.org/techniques/alttext/) :

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <img src={`fiesta.jpg`} alt="A dog smiling in a party hat" /> // highlight-line
)
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement
4. Affichez votre image dans le navigateur: `http://localhost:8000/`

#### Ressources complémentaires

- [Exemple de référencement d'une image du dossier static)](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [Utilisation du dossier static](/docs/static-folder/)
- [En savoir plus sur toutes les techniques d'image dans Gatsby](/docs/images-and-files/)

### Optimisation et requête des images locales avec gatsby-image

Le plugin `gatsby-image` peut soulager une grande partie de la douleur associée à l'optimisation des images sur votre site

Gatsby générera des ressources optimisées qui peuvent être requêtées via GraphQL et transmises dans le composant d'image de Gatsby. Ce composant s’occupe des éléments compliqués de la gestion d’images, y compris la création de plusieurs tailles d'image afin de les charger au bon moment suivant la taille de l’appareil utilisé pour accécer au site.

#### Les prérequis

- Les packages `gatsby-image`, `gatsby-transformer-sharp` et `gatsby-plugin-sharp` installés et ajoutés dans `gatsby-config`
- [Les images accessibles](/packages/gatsby-image/#install) dans votre `gatsby-config` à l'aide d'un plugin comme `gatsby-source-filesystem`

#### Instructions

1. Importez `Img` à partir de `gatsby-image`, ainsi que `graphql` et `useStaticQuery` à partir de `gatsby`

```jsx
import { useStaticQuery, graphql } from "gatsby" // to query for image data
import Img from "gatsby-image" // to take image data and render it
```

2. Écrivez une requête pour obtenir des données d'image, et transmettez ces données dans le composant `<Img />` :

Choisissez l'une des options suivantes ou une combinaison d'entre elles.

a. une seule image interrogée par son [chemin](/docs/content-and-data/) (Exemple : `images/corgi.jpg`) :

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) { // highlight-line
      childImageSharp {
        fluid {
          base64
          aspectRatio
          src
          srcSet
          sizes
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

b. à l'aide d'un [fragment de GraphQL](/docs/using-fragments/), pour interroger les champs nécessaires de façon plus concise :

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid {
          ...GatsbyImageSharpFluid // highlight-line
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

c. plusieurs images d'un répertoire (Exemple : ` images/chiens`) [filtrées](/docs/graphql-reference/#filter) par les champs `extension` et `relativeDirectory`, puis mappées à des composants `Img` :

```jsx
const data = useStaticQuery(graphql`
  query {
    allFile(
      // highlight-start
      filter: {
        extension: { regex: "/(jpg)|(png)|(jpeg)/" }
        relativeDirectory: { eq: "dogs" }
      }
      // highlight-end
    ) {
      edges {
        node {
          base
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`)

return (
  <div>
    // highlight-start
    {data.allFile.edges.map(image => (
      <Img
        fluid={image.node.childImageSharp.fluid}
        alt={image.node.base.split(".")[0]} // utilisez seulement la partie de l’extension comprenant le nom du fichier
      />
    ))}
    // highlight-end
  </div>
)
```

**Note :** Cette méthode peut rendre difficile l'adéquation des images avec le texte alternatif `alt` nécessaire pour l'accessibilité. Cet exemple utilise des images avec du texte `alt` inclus dans le nom de fichier, comme `dog in a party hat.jpg`

d. une image d'une taille fixe utilisant le champ `fixed` au lieu de `fluid` :

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(width: 250, height: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" />
)
```

e. une image d'une taille fixe avec un `maxWidth` :

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fixed(maxWidth: 250) { // highlight-line
          ...GatsbyImageSharpFixed
        }
      }
    }
  }
`)
return (
  <Img fixed={data.file.childImageSharp.fixed} alt="A corgi smiling happily" /> // highlight-line
)
```

f. une image remplissant un container fluide d'une largeur maximale (en pixels) et d'une qualité supérieure (la valeur par défaut est de 50, soit 50 %) :

```jsx
const data = useStaticQuery(graphql`
  query {
    file(relativePath: { eq: "corgi.jpg" }) {
      childImageSharp {
        fluid(maxWidth: 800, quality: 75) { // highlight-line
          ...GatsbyImageSharpFluid
        }
      }
    }
  }
`)

return (
  <Img fluid={data.file.childImageSharp.fluid} alt="A corgi smiling happily" />
)
```

3. (Facultatif) Ajoutez des styles inline à `<Img />`  comme vous le feriez à d'autres composants :

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="A corgi smiling happily"
  style={{ border: "2px solid rebeccapurple", borderRadius: 5, height: 250 }} // highlight-line
/>
```

4. (Facultatif) Forcez une image dans aspect désiré en dominant le champ `aspectRatio` retourné par la requête GraphQL avant qu'elle ne soit passée dans le composant `<Img />` :

```jsx
<Img
  fluid={{
    ...data.file.childImageSharp.fluid,
    aspectRatio: 1.6, // 1280 / 800 = 1.6
  }}
  alt="A corgi smiling happily"
/>
```

5. Exécutez `gatsby develop`, pour générer des images à partir de fichiers (si ce n'est pas déjà fait) et les mettre en cache

#### Ressources complémentaires

- [Exemple de repo illustrant ces exemples](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Gatsby Image API](/docs/gatsby-image/)
- [Utilisation de Gatsby Image](/docs/using-gatsby-image)
- [En savoir plus sur le travail avec des images dans Gatsby](/docs/working-with-images/)
- [Free egghead.io vidéos expliquant ces étapes](https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

### Optimisation et requête des images dans les articles frontmatter avec gatsby-image

Pour les cas d'utilisation comme une image dans un article de blog, vous pouvez utiliser `gatsby-image`. Le composant `Img` nécessite des données d'image traitées, qui peuvent provenir d'un fichier local (ou distant), y compris à partir d'une URL sous la forme d'un fichier `.md` ou `.mdx`.

Pour les images inline en markdown (en utilisant la syntaxe `![]()`), envisagez d'utiliser un plugin comme [`gatsby-remark-images`](/packages/gatsby-remark-images/)

#### Les prérequis

- Les packages `gatsby-image`, `gatsby-transformer-sharp`, et `gatsby-plugin-sharp` installés et ajoutés dans `gatsby-config`
- [Les images accessibles](/packages/gatsby-image/#install) dans votre `gatsby-config` à l'aide d'un plugin comme `gatsby-source-filesystem`
- Des fichiers Markdown accessibles depuis `gatsby-config` avec des URL d'images en frontmatter
- [Pages en Markdown créées](/docs/creating-and-modifying-pages/) en utilisant [`createPages`](https://www.gatsbyjs.org/docs/node-apis/#createPages)

#### Instructions

1. Vérifiez que le fichier Markdown a une URL d'image avec un chemin valide vers un fichier d'image dans votre projet

```mdx:title=post.mdx
---
title: My First Post
featuredImage: ./corgi.png // highlight-line
---

Post content...
```

2. Vérifiez qu'un identifiant unique (une slug dans cet exemple) est passé dans le contexte lorsque `createPages` est appelé dans `gatsby-node.js`, il sera ensuite passé dans une requête GraphQL dans le composant Layout

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }) => {
  const { createPage } = actions

  // query for all markdown

  result.data.allMdx.edges.forEach(({ node }) => {
    createPage({
      path: node.fields.slug,
      component: path.resolve(`./src/components/markdown-layout.js`),
      // highlight-start
      context: {
        slug: node.fields.slug,
      },
      // highlight-end
    })
  })
}
```

3. Maintenant, importez `Img` à partir de `gatsby-image`, et `graphql` à partir de `gatsby` dans le template du composant, écrire une [pageQuery](/docs/page-query/) pour obtenir des données d'image en utilisant le `slug` et transmettre ces données au composant `<Img />`

```jsx:title=markdown-layout.jsx
import React from "react"
import { graphql } from "gatsby" // highlight-line
import Img from "gatsby-image" // highlight-line

export default ({ children, data }) => (
  <main>
    // highlight-start
    <Img
      fluid={data.markdown.frontmatter.image.childImageSharp.fluid}
      alt="A corgi smiling happily"
    />
    // highlight-end
    {children}
  </main>
)

// highlight-start
export const pageQuery = graphql`
  query PostQuery($slug: String) {
    markdown: mdx(fields: { slug: { eq: $slug } }) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluid {
              ...GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
// highlight-end
```

4. Exécutez `gatsby develop`, qui générera des images pour les fichiers accessibles de votre dossier

#### Ressources complémentaires

- [Exemple de repo utilisant cette recette](https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Images avec frontmatter](/docs/working-with-images-in-markdown/#featured-images-with-frontmatter-metadata)
- [Gatsby Image API](/docs/gatsby-image/)
- [Utilisation de Gatsby Image](/docs/using-gatsby-image)
- [En savoir plus sur le travail avec des images dans Gatsby](/docs/working-with-images/)

## 8. Transformer les données

La transformation des données dans Gatsby est axée sur l’utilisation de plugins. Les plugins de transformation prennent les données récupérées à l'aide de plugins sources et les transforment en quelque chose de plus utilisable (par exemple le JSON en objets JavaScript et plus encore)

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

Showtime ! Une fois que vous êtes content de votre site, vous êtes prêt à l’envoyer sur la toile !

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
