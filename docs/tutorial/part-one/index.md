---
title: Apprenez à connaître blocs de construction de Gatsby
typora-copy-images-to: ./
disableTableOfContents: true
---

Dans la [**section précédente**](/tutorial/part-zero/), vous avez préparé votre environnement de développement local en installant les logiciels nécessaires et créé votre premier site Gatsby en utilisant le [**starter “hello world”**](https://github.com/gatsbyjs/gatsby-starter-hello-world). Maintenant, plongeons dans le code généré par le starter.

## Utilisation des starters Gatsby

Dans [**tutoriel partie zéro**](/tutorial/part-zero/), vous avez créé un nouveau site basé sur le starter “hello world” en utilisant la commande suivante :

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

Lors de la création d'un nouveau site Gatsby, vous pouvez utiliser la structure de commande suivante pour créer un nouveau site basé sur tout starter Gatsby existant :

```shell
gatsby new [SITE_DIRECTORY_NAME] [URL_OF_STARTER_GITHUB_REPO]
```

<<<<<<< HEAD
Si vous oubliez l'URL à la fin, Gatsby générera automatiquement un site pour vous en fonction du [**starter par défaut**](https://github.com/gatsbyjs/gatsby-starter-default). Dans cette section du tutoriel, restez avec le site “Hello World” que vous avez déjà créé dans la partie zéro.
=======
If you omit a URL from the end, Gatsby will automatically generate a site for you based on the [**default starter**](https://github.com/gatsbyjs/gatsby-starter-default). For this section of the tutorial, stick with the “Hello World” site you already created in tutorial part zero. You can learn more about [modifying starters](/docs/modifying-a-starter) in the docs.
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

### ✋ Ouvrez le code

Dans votre éditeur, ouvrez le code généré pour votre site “Hello World” et prenez connaissance des différents répertoires et fichiers contenus dans le projet ‘hello-world’. Ça devrait ressembler à quelque chose comme ça :

![Projet Hello World dans VS Code](01-hello-world-vscode.png)

_Note: L'éditeur utilisé est Visual Studio Code. Si vous utilisez un autre éditeur, le résultat sera peut-être différent._

Jetons un coup d’œil au code qui alimente la page d’accueil.

> 💡 Si vous avez arrêté votre serveur de développement après avoir exécuté `gatsby develop` dans la section précédente, redémarrez-le maintenant - il est temps d'apporter quelques modifications au site hello-world !

## Familiarisation avec les pages Gatsby

Ouvrez le répertoire `/src` dans votre éditeur. A l'intérieur s'y trouve un seul dossier : `/pages`.

Ouvrez le fichier `src/pages/index.js`. Le code dans ce fichier crée un composant qui contient une simple div et un peu de texte — à première vue, “Hello world!”

### ✋ Faites des changements dans la page d'accueil “Hello World”

1.  Changez le texte “Hello World!” pour “Hello Gatsby!” et sauvegardez le fichier. Si vos fenêtres sont côte à côte, vous pouvez voir votre code et le contenu qui change de façon presque instantanée dans votre navigateur après avoir sauvegardé le fichier.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./02-demo-hot-reloading.mp4"></source>
  <p>Désolé ! Votre navigateur ne supporte pas cette vidéo.</p>
</video>

> 💡 Gatsby utilise le **rechargement immédiat** pour accélérer votre processus de développement. Lorsque vous exécutez un serveur de développement Gatsby, les fichiers du site Gatsby sont “surveillés” en arrière-plan - chaque fois que vous enregistrez un fichier, vos modifications sont immédiatement reflétées dans le navigateur. Vous n'avez pas besoin d'actualiser la page ou de redémarrer le serveur de développement, vos modifications apparaissent.

2.  Vous pouvez maintenant rendre vos modifications un peu plus visibles. Essayez de remplacer le code dans `src/pages/index.js` avec le code ci-dessous et enregistrez à nouveau. Vous verrez des modifications dans le texte - la couleur du texte sera violette et la taille de la police sera plus grande.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Bonjour Gatsby!</div>
)
```

> 💡 Nous parlerons d'avantage du style dans Gatsby dans [**partie deux**](/tutorial/part-two/) du tutoriel.

3.  Supprimez le style de la taille de la police, changez le texte “Hello Gatsby!” vers une en-tête de premier niveau, et ajoutez un paragraphe en dessous de l'en-tête.

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  {/* highlight-start */}
  <div style={{ color: `purple` }}>
    <h1>Bonjour Gatsby!</h1>
    <p>Quel monde.</p>
  {/* highlight-end */}
  </div>
)
```

![Plus de changements avec le rechargement immédiat](03-more-hot-reloading.png)

4.  Ajoutez une image. (Dans ce cas, une image aléatoire du site Unsplash).

```jsx:title=src/pages/index.js
import React from "react"

export default () => (
  <div style={{ color: `purple` }}>
    <h1>Bonjour Gatsby!</h1>
    <p>Quel monde.</p>
    {/* highlight-next-line */}
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```

![Ajout d'une image](04-add-image.png)

### Attendez… De l'HTML dans notre JavaScript?

_Si vous êtes familier avec React et JSX, n'hésitez pas alors à passer cette section.._ Si vous n’avez jamais utilisé le framework React auparavant, vous vous demandez peut-être ce que HTML fait dans une fonction JavaScript. Ou pourquoi nous importons `react` sur la première ligne, mais ne l’utilisons apparemment pas nulle part. Cette façon hybride «HTML-in-JS» est en fait une extension de syntaxe de JavaScript pour React, appelée JSX. Vous pouvez suivre ce tutoriel sans expérience préalable de React, mais si vous êtes curieux, voici un bref aperçu…

Considérons le contenu original du fichier `src/pages/index.js`:

```jsx:title=src/pages/index.js
import React from "react"

export default () => <div>Hello world!</div>
```

En JavaScript pur, cela ressemble plus à ceci:

```javascript:title=src/pages/index.js
import React from "react"

export default () => React.createElement("div", null, "Hello world!")
```

Vous pouvez maintenant voir l’utilisation de l’import `'react'`! Mais attendez. Vous écrivez en JSX, et non pas en HTML pur et en JavaScript. Comment le navigateur lit-il cela? La réponse courte: ce n’est pas le cas. Les sites Gatsby sont fournis avec des outils déjà configurés pour convertir votre code source en quelque chose que les navigateurs peuvent interpréter.

## Construire avec des composants

La page d'accueil que vous venez de modifier a été créée en définissant un composant de page. Qu'est-ce qu'un "composant"?

Au sens large, un composant est un élément constitutif de votre site. C'est un morceau de code autonome qui décrit une section de l'UI (interface utilisateur).

Gatsby est construit sur React. Lorsque nous parlons d'utiliser et de définir des **composants**, nous parlons en réalité de **composants React** — des morceaux de code autonomes (généralement écrits avec JSX) pouvant accepter des éléments d'entrée et de retourner du React décrivant une section de l'interface utilisateur.

L’un des changements majeurs que vous faites lorsque vous commencez à créer des composants (si vous êtes déjà développeur) est qu’aujourd’hui, votre CSS, HTML et JavaScript sont étroitement couplés et vivent même au sein d'un même fichier.

Bien qu’il s’agisse d’un changement apparemment simple, il a de profondes répercussions sur votre conception de la création de sites web.

Prenons l'exemple de la création d'un bouton personnalisé. Dans le passé, vous créiez une classe CSS (peut-être `.primary-button`) avec vos styles personnalisés, puis vous l'utilisiez chaque fois que vous souhaitiez appliquer ces styles. Par exemple:

```html
<button class="primary-button">Cliquez moi</button>
```

Dans le monde des composants, vous créez plutôt un composant `PrimaryButton` avec vos styles de boutons et les utilisez dans tout votre site, comme ceci:

<!-- prettier-ignore -->
```jsx
<PrimaryButton>Cliquez moi</PrimaryButton>
```

Les composants deviennent les éléments de base de votre site. Au lieu d’être limité aux blocs de construction fournis par le navigateur, par exemple, `<button />`, vous pouvez facilement créer de nouveaux blocs de construction qui répondent aux besoins de vos projets.

### ✋ Utilisation des composants de page

Tout composant React défini dans `src/pages/*.js` deviendra automatiquement une page. Voyons cela en action

Vous avez déjà un fichier `src/pages/index.js` qui vient avec le starter “Hello World”.  starter. Créons une page à propos de.

1.  Créez un nouveau fichier `src/pages/about.js`, copiez le code suivant dans le nouveau fichier, et sauvegardez.

```jsx:title=src/pages/about.js
import React from "react"

export default () => (
  <div style={{ color: `teal` }}>
    <h1>A propos de Gatsby</h1>
    <p>Such wow. Very React.</p>
  </div>
)
```

<<<<<<< HEAD
2.  Accédez à http://localhost:8000/about/.
=======
2.  Navigate to `http://localhost:8000/about/`
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

![Nouvelle page à propos](05-about-page.png)

Il suffit de mettre un composant React dans le fichier `src/pages/about.js`, vous avez une page accessible sur `/about`.

### ✋ Utilisation de sous-composants

Disons que la page d’accueil et la page à propos sont devenues assez grandes et que vous réécriviez beaucoup de choses. Vous pouvez utiliser des sous-composants pour diviser l'interface utilisateur en éléments réutilisables. Les deux de vos pages ont des en-têtes `<h1>` - créez un composant qui sera un `Header`.

1.  Créez un nouveau répertoire dans `src/components` et un fichier, dans ce même dossier, appelé `header.js`.
2.  Ajoutez le code suivant au nouveau fichier `src/components/header.js`.

```jsx:title=src/components/header.js
import React from "react"

export default () => <h1>C'est une en-tête.</h1>
```

3.  Modifiez le fichier `about.js` pour importer le component `Header`. Remplacez le tag `h1` par `<Header />`:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header" // highlight-line

export default () => (
  <div style={{ color: `teal` }}>
    <Header /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Ajout du composant d'en-tête](06-header-component.png)

Dans le navigateur, le texte de l'en-tête “A propos de Gatsby” devrait être remplacé par “C'est une en-tête.” Mais vous ne voulez pas que la page “About” dise “C'est une en-tête.” Vous voulez qu'elle dise, “A propos de Gatsby”.

4.  Retournez dans `src/components/header.js` et faites les changements suivants:

```jsx:title=src/components/header.js
import React from "react"

export default props => <h1>{props.headerText}</h1> {/* highlight-line */}
```

5.  Retournez dans `src/pages/about.js` et faites les changements suivants:

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="A propos de Gatsby" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![Passer des données à l'en-tête](07-pass-data-header.png)

Vous devriez maintenant voir l'en-tête “A propos de Gatsby” encore !

### Que sont les “props”?

<<<<<<< HEAD
Vous avez précédemment défini les composants React comme des éléments de code réutilisables décrivant une interface utilisateur. Pour rendre ces pièces réutilisables dynamiques, vous devez pouvoir leur fournir différentes données. Vous faites cela avec une entrée appelée “props”. Les props sont des propriétés fournies aux composants React.
=======
Earlier, you defined React components as reusable pieces of code describing a UI. To make these reusable pieces dynamic you need to be able to supply them with different data. You do that with input called "props". Props are (appropriately enough) properties supplied to React components.
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

Dans `about.js`, vous avez passé un prop `headerText` avec la valeur de `"A propos de Gatsby"` vers le sous-composant importé `Header` :

```jsx:title=src/pages/about.js
<Header headerText="A propos de Gatsby" />
```

Dans `header.js`, le composant d’en-tête s'attend à recevoir le prop `headerText` (car vous l'aviez écrit dans le composant). Vous pouvez donc y accéder comme ceci :

```jsx:title=src/components/header.js
<h1>{props.headerText}</h1>
```

> 💡 Dans JSX, vous pouvez intégrer n'importe quelle expression JavaScript en l'enveloppant avec `{}`. Voici comment vous pouvez accéder à la propriété (ou “prop!”) `headerText` depuis l'objet “props”.

Si vous aviez passé un autre prop à votre component `<Header />`, comme ceci...

```jsx:title=src/pages/about.js
<Header headerText="A propos de Gatsby" arbitraryPhrase="est facultatif" />
```

...vous auriez pu accéder au prop `arbitraryPhrase` via : `{props.arbitraryPhrase}`.

6.  Pour souligner la façon dont vos composants sont réutilisables, ajoutez un autre component `<Header />` à la page A propos, ajoutez le code suivant dans le fichier `src/pages/about.js`, et sauvegardez.

```jsx:title=src/pages/about.js
import React from "react"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Header headerText="A propos de Gatsby" />
    <Header headerText="C'est vraiment cool" /> {/* highlight-line */}
    <p>Such wow. Very React.</p>
  </div>
)
```

![En-tête en double pour montrer la possibilité de réutilisation](08-duplicate-header.png)

Et voila; Une deuxième tête — sans réécriture de code — en faisant passer des données différentes en utilisant les props.

### Utilisation des composants de présentation

Les composants de mise en page sont destinés aux sections d'un site que vous souhaitez partager sur plusieurs pages. Par exemple, les sites Gatsby auront généralement un composant de présentation avec un en-tête et un pied de page partagés. D'autres éléments courants à ajouter aux mises en page comme une barre latérale et/ou un menu de navigation.

Vous allez explorer les composants de mise en page dans la [**partie trois**](/tutorial/part-three/).

## Liens entre les pages

Vous allez souvent devoir lier des pages entre elles — Regardons d'un peu plus près le routage dans un site Gatsby.

### ✋ Utilisation du composant `<Link />`

1.  Ouvrez le composant page index (`src/pages/index.js`), importez le composant `<Link />` depuis Gatsby, ajoutez un composant `<Link />` au-dessus de l'en-tête, et lui donner une propriété `to` avec la valeur `"/contact/"` pour le chemin :

```jsx:title=src/pages/index.js
import React from "react"
import { Link } from "gatsby" // highlight-line
import Header from "../components/header"

export default () => (
  <div style={{ color: `purple` }}>
    <Link to="/contact/">Contact</Link> {/* highlight-line */}
    <Header headerText="Bonjour Gatsby!" />
    <p>Quel monde.</p>
    <img src="https://source.unsplash.com/random/400x200" alt="" />
  </div>
)
```
Lorsque vous allez cliquer sur le nouveau lien "Contact" sur la page d'accueil, vous devriez voir...

![Gatsby dev 404 page](09-dev-404.png)

...la page 404 de développement de Gatsby. Pourquoi ? Parce que vous essayez d'accéder à une page qui n'existe pas encore.

2.  Maintenant vous allez devoir créer un component de page pour votre nouvelle page "Contact" dans `src/pages/contact.js` et lier la page d'accueil à celui-ci :

```jsx:title=src/pages/contact.js
import React from "react"
import { Link } from "gatsby"
import Header from "../components/header"

export default () => (
  <div style={{ color: `teal` }}>
    <Link to="/">Home</Link>
    <Header headerText="Contact" />
    <p>Envoiez nous un message!</p>
  </div>
)
```

<<<<<<< HEAD
Après avoir enregistré le fichier, vous devriez voir la page de contact et être capable de naviguer entre celle-ci et la page d'accueil.

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Désolé ! Votre navigateur ne supporte pas cette vidéo.</p>
=======
After you save the file, you should see the contact page and be able to follow the link to the homepage.

<video controls="controls" loop="true">
  <source type="video/mp4" src="./10-linking-between-pages.mp4"></source>
  <p>Sorry! Your browser doesn't support this video.</p>
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7
</video>

Le composant Gatsby `<Link />` permet de lier différentes pages de votre site. Pour les liens externes non gérés par votre site Gatsby, utilisez la balise HTML par défaut `<a>`.

## Déployer un site Gatsby

<<<<<<< HEAD
Gatsby.js est un _générateur de site moderne_, ce qui veut dire qu'il n'y a pas besoin de serveurs à configurer ou de base de données à déployer. Au lieu de ça, la commande Gatsby `build` produit un dossier contenant des fichiers HTML statiques ainsi que des fichiers JavaScript que vous pouvez déployer sur n'importe quel hébergeur de site statique.

Essayez d'utiliser [Surge](http://surge.sh/) pour déployer votre premier site Gatsby. Surge est un des nombreux "hébergeurs de site statique" ce qui lui permet de déployer des site Gatsby.
=======
Gatsby.js is a _modern site generator_, which means there are no servers to set up or complicated databases to deploy. Instead, the Gatsby `build` command produces a directory of static HTML and JavaScript files which you can deploy to a static site hosting service.

Try using [Surge](http://surge.sh/) for deploying your first Gatsby website. Surge is one of many "static site hosts" which makes it possible to deploy Gatsby sites.
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

Si vous ne l'aviez pas précédemment installé &amp; installez Surge, ouvrez une nouvelle invite de commande et installez leur outil :

```shell
npm install --global surge

# Et après créez un compte (gratuit) avec eux
surge login
```

Ensuite, construisez votre site en lançant ces commandes dans votre terminal à la racine de votre site (conseil: vérifiez que vous êtes à la racine de votre site, dans ce cas dans le dossier hello-world, ce que vous pouvez faire en un nouvel onglet dans la même fenêtre où vous aviez lancé `gatsby develop`):

```shell
gatsby build
```

La génération devrait prendre 15-30 secondes. Dès que la génération est finie, il est intéressant d'aller voir aux fichiers que la commande `gatsby build` nous a préparé pour le déploiement.

Jetez un coup d'œil aux fichiers générés en tapant cette commande à la racine de votre site, ce qui vous permettra de voir le dossier `public` :

```shell
ls public
```

Finalement, déployez votre site en publiant les fichiers générés sur surge.sh.

```shell
surge public/
```

<<<<<<< HEAD
Dès que la commande est finie, vous devriez voir quelque chose de similaire dans votre invite de commande :
=======
> Note that you will have to press the `enter` key after you see the `domain: some-name.surge.sh` information on your command-line interface.

Once this finishes running, you should see in your terminal something like:
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

![Capture d'écran d'une publication de site Gatsby en utilisant Surge](surge-deployment.png)

Ouvrez l'adresse donnée sur la ligne du bas (`lowly-pain.surge.sh` dans ce cas) et vous allez y voir votre nouveau site ! Bon travail !

## ➡️ Et après ?

Dans cette section, vous :

- Avez découvert les starters de Gatsby, et comment les utiliser pour créer des nouveaux projets
- Avez découvert la syntaxe de JSX
- Avez découvert les composants
- Avez découvert les composants et sous-composants de pages Gatsby
- Avez découvert les “props” React et la réutilisation de composants React

Maintenant, continuez avec [**ajouter des styles à votre site**](/tutorial/part-two/)!
