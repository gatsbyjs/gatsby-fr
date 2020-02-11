---
title: Créer des composants de présentation imbriqués
typora-copy-images-to: ./
disableTableOfContents: true
---

Bienvenue dans cette partie trois !

## Que contient ce tutoriel ?

Dans cette partie vous allez apprendre ce que sont les plugins de Gatsby et comment créer des composants de "présentation" imbriqués.

Les plugins de Gatsby sont des packages JavaScript qui vous aident à rajouter des fonctionnalités à votre site Gatsby. Gatsby a été conçu pour être extensible, ce qui signifie que les plugins sont capables d'étendre ou modifier absolument tout ce que Gatsby peut faire.

Les composants de présentation sont destinés à des parties de votre site que vous voulez partager dans plusieurs pages. Par exemple, les sites vont souvent avoir un composant de présentation avec un en-tête et un pied de page partagé. D'autres choses sont couramment intégrées à des éléments de présentation comme les barres latérales et/ou les menus de navigation. Sur cette page par exemple, l'en-tête fait partie du composant de présentation de gatsby.org.

Attaquons-nous à cette partie trois.

## Utiliser des plugins

Vous êtes très certainement familier avec cette idée de plugins. De nombreux logiciels supportent l'ajout de plugins personnalisés pour ajouter de nouvelles fonctionnalités ou même modifier le fonctionnement interne du logiciel. Les plugins de Gatsby fonctionnent de la même façon.

Des membres de la communauté (comme vous !) peuvent écrire des plugins (une petite quantité de code JavaScript) que d'autres peuvent alors utiliser lors de la création de sites Gatsby.

> Il y a déjà des centaines de plugins ! Explorez la [librairie de plugins](/plugins/).

Notre objectif est de rendre les plugins faciles à installer et à utiliser. Vous allez vraisemblablement utiliser des plugins dans chacun des sites Gatsby que vous allez créer. En travaillant sur le reste du tutoriel, vous aurez de nombreuses occasions de pratiquer l'installation et l'utilisation de plugins.

Pour une première introduction à l'utilisation des plugins nous allons installer et implémenter le plugin Typography.js de Gatsby.

[Typography.js](https://kyleamathews.github.io/typography.js/) est une librairie JavaScript qui génère un style global de base pour la typographie de votre site. La librairie a un [plugin Gatsby correspondant](/packages/gatsby-plugin-typography/) pour faciliter son utilisation dans un site Gatsby.

### ✋ Créer un nouveau site Gatsby

Comme nous l'avons indiqué dans [la partie deux](/tutorial/part-two/), il est sans doute préférable de fermer la ou les fenêtres de votre terminal ainsi que les fichiers des projets des précédentes parties du tutoriel, pour faire le ménage sur votre poste de travail. Ouvrez ensuite une nouvelle fenêtre de terminal et lancez la commande suivante pour créer un nouveau dossier appelé `tutoriel-partie-trois` et entrer dans ce nouveau dossier.

```shell
gatsby new tutoriel-partie-trois https://github.com/gatsbyjs/gatsby-starter-hello-world
cd tutoriel-partie-trois
```

### ✋ Installer et configurer `gatsby-plugin-typography`

Il y a deux étapes principales pour utiliser un plugin : l'installation et la configuration.

1. Installez le paquet NPM `gatsby-plugin-typography`.

```shell
npm install --save gatsby-plugin-typography react-typography typography typography-theme-fairy-gates
```

> Note: Typography.js nécessite quelques paquets additionnels qui sont intégrés dans cette commande. Les besoins additionnels de la sorte seront listés dans la commande "install" de chaque plugin.

2. Modifiez le fichier `gatsby-config.js` à la racine de votre dossier comme suit:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-typography`,
      options: {
        pathToConfigModule: `src/utils/typography`,
      },
    },
  ],
}
```

Le fichier `gatsby-config.js` est un autre de ces fichiers spéciaux que Gatsby va reconnaître automatiquement. C'est à cet endroit que vous ajoutez les plugins et autres configurations de votre site.

> Consultez la [doc sur gatsby-config.js](/docs/gatsby-config/) pour en lire davantage, si vous le souhaitez.

3. Typography.js a besoin d'un fichier de configuration. Créez un nouveau répertoire intitulé `utils` dans le dossier `src`. Ajoutez ensuite un nouveau fichier intitulé `typography.js` dans le dossier `utils` et copiez-y ce qui suit :

```javascript:title=src/utils/typography.js
import Typography from "typography"
import fairyGateTheme from "typography-theme-fairy-gates"

const typography = new Typography(fairyGateTheme)

export const { scale, rhythm, options } = typography
export default typography
```

4. Démarrez votre serveur de développement.

```shell
gatsby develop
```

Lorsque vous chargez votre site, si vous inspectez le fichier HTML généré en utilisant les outils de développement de Chrome, vous constaterez que le plugin typography a ajouté une balise `<style>` dans la balise `<head>` avec le CSS qui a été généré :

![typography-styles](typography-styles.png)

### ✋ Créer du contenu et changer les styles

Copiez ce qui suit dans votre fichier `src/pages/index.js` de sorte à ce que vous puissiez mieux constater les effets sur le CSS généré.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div>
    <h1>
      Salut ! Je suis en train de créer un faux site Gatsby dans le cadre d'un
      tutoriel !
    </h1>
    <p>
      Qu'est ce que j'aime faire ? C'est beaucoup de formations mais j'aime
      vraiment créer des sites web.
    </p>
  </div>
)
```

Votre site devrait ressembler à ça :

![no-layout](no-layout.png)

Faisons une rapide amélioration. De nombreux sites disposent d'une colonne de texte centrée au milieu de la page. Pour créer cela, ajoutez le style suivant à votre `<div>` dans `src/pages/index.js`.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  // highlight-next-line
  <div style={{ margin: `3rem auto`, maxWidth: 600 }}>
    <h1>
      Salut ! Je suis en train de créer un faux site Gatsby dans le cadre d'un
      tutoriel !
    </h1>
    <p>
      Qu'est ce que j'aime faire ? C'est beaucoup de formations mais j'aime
      vraiment créer des sites web.
    </p>
  </div>
)
```

![with-layout2](with-layout2.png)

Super. Vous avez installé et configuré votre tout premier plugin Gatsby !

## Créer un composant de présentation

Passons maintenant à l'apprentissage des composants de présentation. Pour vous préparer à cette partie, ajoutez quelques pages à votre projet : une page à propos et une page de contact.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div>
    <h1>À propos de moi</h1>
    <p>
      Je suis plutôt bon, je suis plutôt intelligent, et bon sang, les gens
      m'aiment !
    </p>
  </div>
)
```

```jsx:title=src/pages/contact.js
import React from "react"

export default () => (
  <div>
    <h1>
      J'aimerais beaucoup discuter ! Envoyez-moi vos messages à cette adresse.
    </h1>
    <p>
      <a href="mailto:me@example.com">me@example.com</a>
    </p>
  </div>
)
```

Regardons à quoi ressemble cette nouvelle page "à propos" :

![about-uncentered](about-uncentered.png)

Mmh. Cela serait bien si le contenu de nos deux nouvelles pages était centré comme sur notre page d'index. Cela serait également bien d'avoir une sorte de navigation globale de manière à ce qu'il soit facile pour les visiteurs de trouver et visiter chaque sous-page.

Vous allez vous attaquer à ces changements en créant votre premier composant de présentation.

### ✋ Créer votre premier composant de présentation

1. Créez un nouveau répertoire `src/components`.

2. Créez un composant de présentation très basique dans `src/components/layout.js`:

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {children}
  </div>
)
```

3. Importez ce nouveau composant de présentation dans votre composant de page `src/pages/index.js` :

```jsx:title=src/pages/index.js
import React from "react"
import Layout from "../components/layout" // highlight-line

export default () => (
  <Layout> {/* highlight-line */}
    <h1>Salut ! Je suis en train de créer un faux site Gatsby dans le cadre d'un
              tutoriel !</h1>
    <p>
      Qu'est ce que j'aime faire ? C'est beaucoup de formations mais j'aime vraiment créer des
            sites web.
    </p>
  </Layout> {/* highlight-line */}
)
```

![with-layout2](with-layout2.png)

Super. La présentation marche ! Le contenu de votre page d'accueil est toujours centré.

Mais essayez d'aller sur `/about/`, ou `/contact/`. Le contenu de ces pages n'est toujours pas centré.

4. Importez le composant de mise en page dans `about.js` et `contact.js` (comme vous l'avez fait dans `index.js` à l'étape précédente).

Le contenu de vos trois pages est maintenant centré grâce à cet unique composant de présentation partagé!

### ✋ Ajouter un titre au site

1. Ajoutez la ligne suivante dans votre composant de présentation :

```jsx:title=src/components/layout.js
import React from "react"

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    <h3>MonSuperSite</h3> {/* highlight-line */}
    {children}
  </div>
)
```

Si vous allez dans chacune de vos trois pages, vous verrez que le même titre a été ajouté, comme par exemple dans la page `/about/` :

![with-title](with-title.png)

### ✋ Ajouter des liens de navigation entre les pages

1. Copiez ce qui suit dans votre composant de présentation :

```jsx:title=src/components/layout.js
import React from "react"
// highlight-start
import { Link } from "gatsby"

const ListLink = props => (
  <li style={{ display: `inline-block`, marginRight: `1rem` }}>
    <Link to={props.to}>{props.children}</Link>
  </li>
)
// highlight-end

export default ({ children }) => (
  <div style={{ margin: `3rem auto`, maxWidth: 650, padding: `0 1rem` }}>
    {/* highlight-start */}
    <header style={{ marginBottom: `1.5rem` }}>
      <Link to="/" style={{ textShadow: `none`, backgroundImage: `none` }}>
        <h3 style={{ display: `inline` }}>MySweetSite</h3>
      </Link>
      <ul style={{ listStyle: `none`, float: `right` }}>
        <ListLink to="/">Accuei</ListLink>
        <ListLink to="/about/">À propos</ListLink>
        <ListLink to="/contact/">Contact</ListLink>
      </ul>
    </header>
    {/* highlight-end */}
    {children}
  </div>
)
```

![with-navigation2](with-navigation.png)

Et voilà ! Maintenant vous avez un site de trois pages avec un menu de navigation de base.

<<<<<<< HEAD
_Challenge:_ Avec vos nouveaux pouvoirs de "composants de présentation", essayez d'ajouter à votre site Gatsby des en-têtes, pieds de page, menus de navigation, barres latérales, etc.
=======
_Challenge:_ With your new "layout component" powers, try adding headers, footers, global navigation, sidebars, etc. to your Gatsby sites!
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

## Quelle est la suite ?

Continuez sur la [partie quatre de ce tutoriel](/tutorial/part-four/) où vous commencerez à en apprendre plus sur la couche de données de Gatsby et la création programmatique de pages !
