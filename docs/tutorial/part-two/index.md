---
title: Introduction à la mise en page avec Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

<!-- Idea: Create a glossary to refer to. A lot of these terms get jumbled -->

<!--
  - Global styles
  - Component css
  - CSS-in-JS
  - CSS Modules

-->

Bienvenue dans la partie deux de ce tutoriel Gatsby !

## Que contient ce tutoriel ?

Dans cette partie, vous allez explorer les différentes options à votre disposition pour mettre en page un site Gatsby et vous irez plus loin dans l'utilisation des composants React pour créer des sites.

## Utiliser des styles globaux

Tout site dispose d'une sorte de style de mise en page globale. Cela peut inclure des choses comme la typographie et les couleurs de fond. Ces styles servent à créer l'ambiance générale du site — comme la couleur et la texture d'un mur donne une ambiance générale à une pièce.

### Créer des styles globaux avec des fichiers CSS classiques

La manière la plus simple d'ajouter des styles globaux est d'utiliser une feuille de style globale en `.css`

#### ✋ Créer un nouveau site Gatsby

Commencez par créer un nouveau site Gatsby. Il est peut-être judicieux (particulièrement si vous êtes débutant dans l'utilisation des lignes de commandes) de fermer la fenêtre du terminal que vous avez utilisé pour la [première partie](/tutorial/part-one/) et démarrer une nouvelle instance du terminal.

Ouvrez une nouvelle fenêtre de terminal, créez un nouveau site "hello world" avec Gatsby dans un dossier appelé `tutorial-part-two`, ensuite déplacez-vous dans ce dossier :

```shell
gatsby new tutoriel-partie-deux https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutorial-part-two
```

Vous avez maintenant un nouveau site Gatsby (basé sur le kit de démarrage "hello world") avec la structure suivante:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
```

<<<<<<< HEAD
#### ✋ Ajoutez des styles à votre fichier `.css`
=======
#### ✋ Add styles to a CSS file
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097

1. Créez un fichier `.css` dans votre nouveau projet :

```shell
cd src
mkdir styles
cd styles
touch global.css
```

> Note : Vous êtes libre de créer ces dossiers et ces fichiers en utilisant votre éditeur de code si vous préférez.

Maintenant, la structure de votre site doit être la suivante:

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
```

2. Définissez quelques styles dans le fichier `global.css` :

```css:title=src/styles/global.css
html {
  background-color: lavenderblush;
}
```

<<<<<<< HEAD
> Note : L'emplacement de ce fichier css d'exemple (dans le dossier`/src/styles/`) est purement arbitraire.
=======
> Note: The placement of the example CSS file in a `/src/styles/` folder is arbitrary.
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097

#### ✋ Inclure votre feuille de style dans `gatsby-browser.js`

1. Créez le fichier `gatsby-browser.js`

```shell
cd ../..
touch gatsby-browser.js
```

La structure de votre projet devrait maintenant ressembler à ça :

```text
├── package.json
├── src
│   └── pages
│       └── index.js
│   └── styles
│       └── global.css
├── gatsby-browser.js
```

> 💡 Qu'est-ce que `gatsby-browser.js`? Ne vous préoccupez pas trop de ça pour l'instant. Sachez simplement que `gatsby-browser.js` est l'un des quelques fichiers spéciaux que Gatsby recherche et utilise (s'ils existent). Ici, le nom du fichier **est** important. Si vous voulez en apprendre davantage maintenant, jetez un oeil à [la documentation](/docs/browser-apis/).

2. Importez votre toute nouvelle feuille de style dans le fichier `gatsby-browser.js` :

```javascript:title=gatsby-browser.js
import "./src/styles/global.css"

// or:
// require('./src/styles/global.css')
```

> Note : Les deux syntaxes (`require`) de CommonJS et (`import`) des modules ES fonctionnent ici. Si vous n'êtes pas sûr de laquelle choisir, `import` est la plupart du temps un bon choix par défaut. Cependant, lorsque vous travaillez sur des fichiers qui ne sont exécutés que dans un evironnement Node.js (comme `gatsby-node.js`), `require` devra être utilisé.

3. Lancez le serveur de développement:

```shell
gatsby develop
```

Si vous jetez un oeil à votre projet dans votre navigateur, vous devriez voir un fond de page couleur lavande dans votre kit de démarrage.

![Hello World Lavande!](global-css.png)

> Astuce : Cette partie du tutoriel s'est concentrée sur la façon la plus rapide et la plus simple de commencer à mettre en page un site Gatsby — en important une feuille de style classique dans `gatsby-browser.js`. Dans la plupart des cas, la meilleure façon de rajouter des styles globaux est d'utiliser un composant de mise en page partagé. [Regardez la documentation](/docs/global-css/) pour plus d'information sur cette méthode.

## Utiliser du CSS limité à un composant

<<<<<<< HEAD
Jusqu'à maintenant nous avons parlé de la méthode la plus traditionnelle en utilisant une feuille de style CSS classique. Maintenant, nous allons parler des diverses méthodes pour modulariser du CSS et appréhender les mises en page du point de vue des composants.
=======
So far, we've talked about the more traditional approach of using standard CSS stylesheets. Now, we'll talk about various methods of modularizing CSS to tackle styling in a component-oriented way.
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097

### Modules CSS

À la découverte des **Modules CSS**. Citation tirée de
[la page d'accueil des modules CSS](https://github.com/css-modules/css-modules):

> Un **Module CSS** est un fichier CSS où toutes les classes et animations ont une portée locale par défaut.

Les Modules CSS sont très populaires parce qu'ils vous permettent d'écrire du CSS normalement, mais avec plus de sûreté. Cet outil génère automatiquement des noms de classes et d'animations uniques, de sorte que vous n'ayez pas à vous soucier de conflits dans vos sélecteurs.

Gatsby fonctionne directement avec les Modules CSS. Cette approche est véritablement recommandée à tous ceux qui utilisent Gatsby (et React en général) depuis peu de temps.

#### ✋ Créer une nouvelle page avec les Modules CSS

Dans cette section vous allez créer une nouvelle page sous forme de composant et la mettre en page en utilisant les Modules CSS.

Tout d'abord, créez un nouveau composant `Container`.

1. Créez un nouveau dossier `src/components` et créez-y un fichier `container.js` en copiant le contenu suivant:

```jsx:title=src/components/container.js
import React from "react"
import containerStyles from "./container.module.css"

export default ({ children }) => (
  <div className={containerStyles.container}>{children}</div>
)
```

<<<<<<< HEAD
Comme vous pouvez le remarquer, vous avez importé un module CSS du nom de `container.module.css`. Créons ce fichier maintenant.
=======
You'll notice you imported a CSS module file named `container.module.css`. Let's create that file now.
>>>>>>> 8ff6bb09c23261662f47e79a041a92855d517097

2. Dans ce même dossier (`src/components`), créez le fichier `container.module.css` et copiez le contenu suivant :

```css:title=src/components/container.module.css
.container {
  margin: 3rem auto;
  max-width: 600px;
}
```

Vous remarquerez que le nom du fichier ce termine par `.module.css` à la place du traditionnel `.css`. C'est de cette manière que vous indiquez à Gatsby que votre fichier CSS doit être traité comme un Module CSS plutôt que comme un fichier CSS standard.

3. Créez une nouvelle page sous forme de composant en créant ce fichier :
   `src/pages/about-css-modules.js`:

```jsx:title=src/pages/about-css-modules.js
import React from "react"

import Container from "../components/container"

export default () => (
  <Container>
    <h1>A propos des Modules CSS</h1>
    <p>Les Modules CSS sont cools</p>
  </Container>
)
```

Si vous visitez maintenant `http://localhost:8000/about-css-modules/`, votre page devrait ressembler à ça :

![page mise en page avec les Modules CSS](css-modules-basic.png)

#### ✋ Mettre en page un composant avec les Modules CSS.

Dans cette section, vous allez créer une liste de personnes avec des noms, avatars, et une courte biographie latine. Vous allez créer un composant `<User />` et le mettre en page avec un Module CSS

1. Créez le fichier pour le CSS ici : `src/pages/about-css-modules.module.css`.

2. Copiez le contenu suivant dans ce nouveau fichier :

```css:title=src/pages/about-css-modules.module.css
.user {
  display: flex;
  align-items: center;
  margin: 0 auto 12px auto;
}

.user:last-child {
  margin-bottom: 0;
}

.avatar {
  flex: 0 0 96px;
  width: 96px;
  height: 96px;
  margin: 0;
}

.description {
  flex: 1;
  margin-left: 18px;
  padding: 12px;
}

.username {
  margin: 0 0 12px 0;
  padding: 0;
}

.excerpt {
  margin: 0;
}
```

3. Importez votre nouveau fichier `src/pages/about-css-modules.module.css` dans la page `about-css-modules.js` que vous aviez créé plus tôt en éditant les premières lignes du fichier comme suit :

```javascript:title=src/pages/about-css-modules.js
import React from "react"
// highlight-next-line
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

// highlight-next-line
console.log(styles)
```

Le code `console.log(styles)` va logger le résultat de cet import de sorte que vous puissiez voir le résultat généré par le fichier `./about-css-modules.module.css`. Si vous ouvrez la console développeur (en utilisant par exemple l'outil de développement de Firefox ou de Chrome, on y accède souvent par la touche F12) dans votre navigateur, vous verrez :

![le résultat de votre import du Module CSS](css-modules-console.png)

Si vous comparez ce résultat avec votre fichier CSS, vous verrez que chaque classe dans l'objet que vous importez pointe vers une longue chaine de caractères. Par exemple: `avatar` pointe vers `src-pages----about-css-modules-module---avatar---2lRF7`. Ce sont les noms de classes générés par les Modules CSS. Ils garantissent leur unicité à travers tout votre site. Étant donné que vous les importez pour les utiliser, il n'y a jamais de doute quant à savoir où s'applique votre CSS.

4. Créez un nouveau composant `<User />` dans votre composant de page `about-css-modules.js`. Modifiez `about-css-modules.js` afin qu'il ressemble à ça :

```jsx:title=src/pages/about-css-modules.js
import React from "react"
import styles from "./about-css-modules.module.css"
import Container from "../components/container"

console.log(styles)

// highlight-start
const User = props => (
  <div className={styles.user}>
    <img src={props.avatar} className={styles.avatar} alt="" />
    <div className={styles.description}>
      <h2 className={styles.username}>{props.username}</h2>
      <p className={styles.excerpt}>{props.excerpt}</p>
    </div>
  </div>
)
// highlight-end

export default () => (
  <Container>
    <h1>About CSS Modules</h1>
    <p>CSS Modules are cool</p>
    {/* highlight-start */}
    <User
      username="Jane Doe"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/adellecharles/128.jpg"
      excerpt="I'm Jane Doe. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    <User
      username="Bob Smith"
      avatar="https://s3.amazonaws.com/uifaces/faces/twitter/vladarbatov/128.jpg"
      excerpt="I'm Bob Smith, a vertically aligned type of guy. Lorem ipsum dolor sit amet, consectetur adipisicing elit."
    />
    {/* highlight-end */}
  </Container>
)
```

> Astuce: généralement, si vous utilisez un composant à de nombreux endroits sur votre site, il devrait être placé dans son propre fichier dans le dossier `components`. Cependant, s'il n'est utilisé que dans un fichier, créez-le directement dans ce dernier.

Votre page terminée devrait ressembler à ça:

![page listant les utilisant avec les Modules CSS](css-modules-userlist.png)

### CSS-dans-JS

CSS-dans-JS est une approche de mise en page orientée sur les composants. La plupart du temps, c'est une méthode où [le CSS est écrit directement dans le fichier JavaScript](https://reactjs.org/docs/faq-styling.html#what-is-css-in-js).

#### Utiliser CSS-dans-JS avec Gatsby

Il y a beaucoup de librairies de CSS-dans-JS et nombre d'entre elles dispose déjà d'un plug-in Gatsby. Nous ne traiterons pas d'un exemple de CSS-dans-JS dans ce tutoriel de base, mais nous vous encourageons à [explorer](/docs/styling/) ce que cet écosystème a à offrir. Il y a des mini tutoriels pour deux librairies en particulier [Emotion](/docs/emotion/) et [Styled Components](/docs/styled-components/).

#### Suggestion de lecture sur CSS-dans-JS

Si vous souhaitez en lire plus à ce sujet, regardez [la présentation de Christopher "vjeux" Chedeau en 2014 qui a initié ce mouvement](https://speakerdeck.com/vjeux/react-css-in-js) et aussi [le récent article de Mark Dalgleish "A Unified Styling Language"](https://medium.com/seek-blog/a-unified-styling-language-d0c208de2660).

### Other CSS options

Gatsby supporte pratiquement toutes les options de mise en page possible (s'il n'y a pas encore de plug-in pour votre méthode CSS favorite [s'il vous plaît, créez-en un!](/contributing/how-to-contribute/))

- [Typography.js](/packages/gatsby-plugin-typography/)
- [Sass](/packages/gatsby-plugin-sass/)
- [JSS](/packages/gatsby-plugin-jss/)
- [Stylus](/packages/gatsby-plugin-stylus/)
- [PostCSS](/packages/gatsby-plugin-postcss/)

Et plus !

## Quelle est la suite ?

Continuez maintenant sur [la partie trois de ce tutoriel](/tutorial/part-three/), où vous en apprendrez vous sur les plug-ins Gatsby et les composants de mise en page.
