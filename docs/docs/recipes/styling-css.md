---
title: "Recettes: Styliser avec du CSS"
tableOfContentsDepth: 1
---
Il y différentes façons d'ajouter du style à votre site internet; Gatsby supporte presque toutes les options possibles grâce à des plugins officiels ou communautaires.

## Utiliser des feuilles de styles CSS globales sans de composant Layout

### Prérequis
- Un [site Gatsby](/docs/quick-start/) existant avec un composant page index
- Un fichier `gatsby-browser.js`

### Instructions
1. Créez une feuille de style CSS globale `src/styles/global.css` et y coller les lignes suivantes

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}

p {
  color: maroon;
}
```
2. Importer la feuille de style CSS globale dans le fichier `gatsby-browser.js` comme suivant:

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"
```

> **Note:** Vous pouvez aussi utiliser `require('./src/styles/global.css')` pour importer la feuille de style globale dans votre fichier gatsby-config.js`

3. Lancez `gatsby-develop` pour observer le style global appliqué à votre site.

> **Note:** Cette approche n'est pas la meilleure si vous utilisez du CSS-in-JS pour styliser votre site, dans quel cas une page layout avec tous les composants partagés doit être utilisée. Cette approche est couverte dans la prochaine recette.

### Ressources additionnelles

- Plus d'informations sur comment [ajouter un style global sans composant layout](/docs/global-css/#adding-global-styles-without-a-layout-component)

## Utiliser des styles globaux dans un composant layout

### Prérequis

- Un [site Gatsby](/docs/quick-start/) existant avec un composant page index

### Instructions

Vous pouvez ajouter des styles globaux à un [composant layout partagé](/tutorial/part-three/#your-first-layout-component). Ce composant est utiliser pour des choses communes sur le site, comme le header ou le footer.

1. Si vous n'en avez pas encore un, créez un nouveau répertoire dans votre projet en tant que `/src/components`.

2. Dans le répertoire components, créez deux fichiers: `layout.css` et `layout.js`.

3. Ajouter ce qui suit à `layout.css`:

```css:title=/src/components/layout.css
body {
  background: red;
}
```

4. Éditer `layout.js` pour importer le fichier CSS sortir le layout markup:

```jsx:title=/src/components/layout.js
import React from "react"
import "./layout.css"

export default ({ children }) => <div>{children}</div>
```

5. Maintenant, éditez la page d'accueil de votre site `/src/pages/index.js` et utilisez le nouveau composant layout:

```jsx:title=/src/pages/index.js
import React from "react"
import Layout from "../components/layout"

export default () => <Layout>Hello world!</Layout>
```

### Ressources additionnelles

- [Style standard avec des fichiers CSS globaux](/docs/global-css/)
- [Standard Styling with Global CSS Files](/docs/global-css/)
- [En savoir plus sur les composants layout](/tutorial/part-three)

## Utiliser Styled Components

### Prérequis

- Un [site Gatsby](/docs/quick-start/) existant avec un composant page index
- [gatsby-plugin-styled-components, styled-components, and babel-plugin-styled-components](/packages/gatsby-plugin-styled-components/) installés dans `package.json`

### Instructions

1. Dans votre fichier `gatsby-config.js` ajoutez `gatsby-plugin-styled-components`

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-styled-components`],
}
```

2. Ouvrir le composant index de page (`src/pages/index.js`) et importer le paquet `styled-components`

3. Stylisez les composants en créant des blocks de style pour chaque type d'élément

4. Appliquez à la page en incluant styled components dans le JSX

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

4. Lancer `gatsby develop` pour voir les changements

### Ressources additionnelles

- [En savoir plus sur l'utilisation de composants stylisés](/docs/styled-components/)
- [Leçon sur Egghead](https://egghead.io/lessons/gatsby-style-gatsby-sites-with-styled-components)

## Utiliser les CSS Modules

### Prérequis

- Un [site Gatsby](/docs/quick-start/) existant avec un composant page index

### Instructions

1. Créez un module CSS en tant que `src/pages/index.module.css` et collez ce qui suit dans le module:

```css:title=src/pages/index.module.css
.feature {
  margin: 2rem auto;
  max-width: 500px;
}
```

2. Importez le module CSS en tant qu'objet JSX `style` dans le fichier `index.js` en modifiant la page pour qu'elle ressemble à ce qui suit:

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

3. Lancez `gatsby develop` pour voir les changements.

**Note:**
Notez que l'extension de fichier est `.module.css` au lieu de `.css`, ce qui indique à Gatsby qu'il s'agit d'un module CSS.

### Ressources additionnelles

- Plus d'informations sur [utiliser les CSS Modules](/tutorial/part-two/#css-modules)
- [Exemple sur l'utilisation des modules CSS](https://github.com/gatsbyjs/gatsby/blob/master/examples/using-css-modules)

## Utiliser Sass/SCSS

Sass est une extension de CSS qui vous offre des fonctionnalités plus avancées telles que des règles imbriquées, des variables, des mixins, etc.

Sass has 2 syntaxes. The most commonly used syntax is "SCSS", and is a superset of CSS. That means all valid CSS syntax, is valid SCSS syntax. SCSS files use the extension `.scss`

Sass a 2 syntaxes. La syntaxe la plus utilisée est "SCSS" et est un sur-ensemble de CSS. Cela signifie que toute syntaxe CSS valide est une syntaxe SCSS valide. Les fichiers SCSS utilisent l'extension `.scss`

### Prérequis

- Un [site Gatsby](/docs/quick-start/).

### Instructions

1. Installez le plugin Gatsby [gatsby-plugin-sass](https://www.gatsbyjs.org/packages/gatsby-plugin-sass/) and `node-sass`

`npm install --save node-sass gatsby-plugin-sass`

2. Incluez le plugin dans votre fichier `gatsby-config.js`.

```javascript:title=gatsby-config.js
plugins: [`gatsby-plugin-sass`],
```

3.  Écrivez vos feuilles de style en tant que fichiers `.sass` ou `.scss` et importez-les. Si vous ne savez pas comment importer des styles, jetez un œil à [Styliser avec CSS](/docs/recipes/#2-styling-with-css).

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

_Note: Vous pouvez également utiliser des fichiers Sass / SCSS comme modules, comme mentionné dans la recette précédente sur les modules CSS, à la différence qu'au lieu de `.css` les extensions doivent être `.scss` ou `.sass`Styling avec CSS_

### Ressources additionnelles

- [Différences entre .sass and .scss](https://responsivedesign.is/articles/difference-between-sass-and-scss/)
- [Guide Sass sur le site officiel de Sass](https://sass-lang.com/guide)
- [Un tutoriel d'installation plus complet sur Sass avec plus d'explications et plus de ressources](https://www.gatsbyjs.org/docs/sass/)

## Ajout d'une police locale

### Prérequis

- Un [site Gatsby](/docs/quick-start/)
- Un fichier police: `.woff2`, `.ttf`, etc.

### Instructions

1. Copiez un fichier de police dans votre projet Gatsby, tel que `src/fonts/fontname.woff2`.

```text
src/fonts/fontname.woff2
```

2. Importez l'élément de police dans un fichier CSS pour l'intégrer à votre site Gatsby:

```css:title=src/css/typography.css
@font-face {
  font-family: "Font Name";
  src: url("../fonts/fontname.woff2");
}
```

**Note:** Assurez-vous que le nom de la police est référencé à partir du CSS approprié, par exemple:

```css:title=src/components/layout.css
body {
  font-family: "Font Name", sans-serif;
}
```

En ciblant l'élément HTML `body`, votre police s'appliquera à la plupart du texte de la page. Des CSS supplémentaires peuvent cibler d'autres éléments, tels que `button` ou `textarea`.

Si les polices ne sont pas mises à jour en suivant les étapes ci-dessus, assurez-vous de remplacer la famille de polices existante dans la feuille de style CSS correspondante.

### Ressources additionnelles

- Plus d'informations sur [importation d'assets dans des fichiers](/docs/importing-assets-into-files/)

## Utiliser Emotion

[Emotion](https://emotion.sh) est une puissante bibliothèque CSS-in-JS qui prend en charge à la fois les styles CSS en ligne et les composants stylisés. Vous pouvez utiliser chaque fonction de style individuellement ou ensemble dans le même fichier.

### Prérequis

- Un [site Gatsby](/docs/quick-start)

### Instructions

1. Installer le plugin [Gatsby Emotion](/packages/gatsby-plugin-emotion/) et les paquets Emotion.

```shell
npm install --save gatsby-plugin-emotion @emotion/core @emotion/styled
```

2. Ajoutez le plugin `gatsby-plugin-emotion` à votre fichier `gatsby-config.js`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-emotion`],
}
```

3. Si vous n'en avez pas déjà, créez une page dans votre site Gatsby à l'adresse `src/pages/emotion-sample.js`.

Importez le paquet de base CSS d'Emotion'. Vous pouvez ensuite utiliser l'accessoire `css` pour ajouter des [styles d'objet Emotion](https://emotion.sh/docs/object-styles) à tout élément à l'intérieur d'un composant:

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

4. Pour utiliser les [composants stylisés](https://emotion.sh/docs/styled) d'Emotion, importez le package et les définir à l'aide de la fonction `styled`.

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

### Ressources additionnelles

- [Utiliser Emotion dans Gatsby](/docs/emotion/)
- [Site d'Emotion](https://emotion.sh)
- [Débuter avec Emotion et Gatsby](https://egghead.io/lessons/gatsby-getting-started-with-emotion-and-gatsby)

## Utiliser les Google Fonts

L'hébergement de vos propres [Google Fonts] (https://fonts.google.com/) localement dans un projet signifie qu'elles n'auront pas à être récupérées sur le réseau lors du chargement de votre site, augmentant l'indice de vitesse de votre site jusqu'à ~ 300 millisecondes sur le bureau et 1+ secondes sur la 3G. Il est également recommandé de limiter l'utilisation des polices personnalisées à l'essentiel pour les performances.

### Prérequis

- Un [site Gatsby](/docs/quick-start)
- L'outil en [ligne de commande de Gatsby](/docs/gatsby-cli/) installé
- Choisir un package de polices dans [le projet de polices de caractères](https://github.com/KyleAMathews/typefaces)

### Instructions

1. Lancez `npm install --save typeface-votre-police-de-caractere`, remplacez `votre-police-de-caractere` avec le nom de la police que vous voulez installer depuis [le projet de polices de caractères](https://github.com/KyleAMathews/typefaces)

Un exemple pour charger la police populaire 'Source Sans Pro' serait:: `npm install --save typeface-source-sans-pro`.

2. Ajoutez `import "typeface-your-chosen-font"` au template layout, au composant page ou à `gatsby-browser.js`.

```jsx:title=src/components/layout.js
import "typeface-your-chosen-font"
```

3. Une fois importé, vous pouvez référencer le nom de la police dans une feuille de style CSS, un module CSS ou CSS-in-JS

```css:title=src/components/layout.css
body {
  font-family: "Your Chosen Font";
}
```

_NOTE: Ainsi, pour l'exemple ci-dessus, la déclaration CSS pertinente serait `font-family: 'Source Sans Pro';`_

### Ressources additionnelles

- [Typography.js](/docs/typography-js/) - Une autre option pour utiliser les polices Google sur un site Gatsby
- [Documentation du projet sur les polices de caractères](https://github.com/KyleAMathews/typefaces/blob/master/README.md)
- [Exemple sur le blog de Kyle Mathews](https://www.bricolage.io/typefaces-easiest-way-to-self-host-fonts/)

## Utiliser Font Awesome

Utiliser [Font Awesome](https://fontawesome.com/) vous donne accès à des milliers d'icônes à utiliser sur votre site. Étant donné que les sites Gatsby sont des sites React, il est recommandé d'utiliser la bibliothèque SVG [react-fontawesome](https://github.com/FortAwesome/react-fontawesome).

### Prérequis

- L'outil en [ligne de commande Gatsby](/docs/gatsby-cli/) installé
- Un [site Gatsby](/docs/quick-start)

### Instructions

1. Installez les dépendances `react-fontawesome`.

```shell
npm install @fortawesome/fontawesome-svg-core  @fortawesome/free-brands-svg-icons @fortawesome/react-fontawesome
```

> Notez qu'il existe plusieurs bibliothèques d'icônes dans `react-fontawesome`. Vous pourriez également être intéressé par les icônes `free-regular-svg-icons` et `free-solid-svg-icons` que vous installeriez de la même manière.

2. Importez le composant `FontAwesomeIcon` et l'icône que vous souhaitez utiliser. Utilisez ensuite l'icône comme un composant directement dans votre ficher JSX:

```jsx:title=index.js
import { FontAwesomeIcon } from "@fortawesome/react-fontawesome"
import { faReact } from "@fortawesome/free-brands-svg-icons"

const IndexPage = () => (
  <Layout>
    <FontAwesomeIcon icon={faReact} /> //highlight-line
  </Layout>
)

export default IndexPage
```

> Cet exemple importe une seule icône spécifique et l'utilise pour de meilleures performances. Comme alternative, vous pouvez [importer les icônes et créer une bibliothèque](https://github.com/FortAwesome/react-fontawesome#build-a-library-to-reference-icons-throughout-your-app-more-conveniently).

### Ressources additionnelles

- [Font Awesome](https://fontawesome.com/)
- [react-fontawesome](https://github.com/FortAwesome/react-fontawesome)
