---
title: Link API Link de Gatsby
---

Pour la navigation interne, entre les pages d'un site, Gatsby dispose d'un composant `<Link>`, ainsi que de la fonction `navigate` utilisée pour la "navigation programmatique" _(NdT: sans action de l'utilisateur)_.

Le composant `<Link>` de Gatsby permet de relier entre elles des pages internes; il permet également de bénéficier d'une fonctionnalité puissante appelée "préchargement" _(NdT: preloading)_. Le préchargement est utilisé pour demander des ressources en avance de sorte que ces ressources seront disponibles lorsque l'utilisateur naviguera effectivement vers elles. Nous utilisons pour cela un `IntersectionObserver` pour envoyer une requête de faible priorité lorsqu'un `Link` est visible, puis nous utilisons un événement `onMouseOver` pour déclencher une requête de haute priorité lorsqu'il est plus que probable que l'utilisateur souhaite naviguer vers la ressource demandée.

Le composant `<Link>` de Gatsby est un wrapper autour du [composant Link de @reach/router](https://reach.tech/router/api/Link) et complète ses fonctionnalités par des ajouts bien utiles spécifiques à Gatsby. Toutes les props passées au `Link` de Gatsby seront transmises au composant `Link` de @reach/router.

## Comment utiliser Gatsby Link

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-why-and-how-to-use-gatsby-s-link-component"
  lessonTitle="Pourquoi et Comment utiliser le composant Link de Gatsby"
/>

### Pour les liens locaux, remplacer les balises `a` par des `Link` 

Pour toutes les situations où vous souhaitez créer des liens entre différentes pages d'un même site, utilisez le composant `Link` à la place d'une balise `a`.

```jsx
import React from "react"
// highlight-next-line
import { Link } from "gatsby"

const Page = () => (
  <div>
    <p>
      {/* highlight-next-line */}
      Consultez mon <Link to="/blog">blog</Link>!
    </p>
    <p>
      {/* Notez que les liens externes devront toujours utiliser une balise `a`. */}
      Suivez-moi sur <a href="https://twitter.com/gatsbyjs">Twitter</a>!
    </p>
  </div>
)
```

### Définissez des styles personnalisés pour les liens actuellement actifs

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-add-custom-styles-for-the-active-link-using-gatsby-s-link-component"
  lessonTitle="Définissez des styles personnalisés pour les liens actifs en utilisant le composant Link de Gatsby"
/>

C'est souvent une bonne idée de montrer quelle page est actuellement visualisée en changeant le rendu visuel du lien qui correspond à cette page.

`Link` fournit deux moyens d'ajouter des styles au lien actif:

- `activeStyle` — un objet contenant des informations de style qui ne sont appliquées que lorsque l'élément courant est actif
- `activeClassName` — un nom de classe CSS qui n'est ajouté au `Link` que lorsque l'élément courant est actif

Par exemple, pour colorer en rouge le lien actif, vous pouvez utiliser l'une des approches suivantes:

```jsx
import React from "react"
import { Link } from "gatsby"

const SiteNavigation = () => (
  <nav>
    <Link
      to="/"
      {/* highlight-start */}
      {/* On suppose ici qu'une classe `active` est définie dans votre CSS */}
      activeClassName="active"
      {/* highlight-end */}
    >
      Accueil
    </Link>
    <Link
      to="/about/"
      {/* highlight-next-line */}
      activeStyle={{ color: "red" }}
    >
      À propos
    </Link>
  </nav>
)
```

### Utiliser `getProps` pour une customisation avancée des styles des liens

Le composant `<Link>` de Gatsby dispose d'une prop `getProps`, qui peut être bien utile pour une customisation avancée des styles. Cette méthode vous transmettra un objet ayant les propriétés suivantes:

- `isCurrent` — vrai si `location.pathname` est strictement ègal à la prop `to` du composant `<Link>`
- `isPartiallyCurrent` — vrai si `location.pathname` commence par la valeur de la prop `to` du composant `<Link>`
- `href` — la valeur de la prop `to`
- `location` — l'attribut `location` de la page

Vous pouvez en apprendre davantage en vous référant à la [`documentation de @reach/router`](https://reach.tech/router/api/Link).

### Afficher les styles actifs pour des liens partiellement compatibles et des liens parents

Par défaut, pour un composant `<Link>` donné, les props `activeStyle` et `activeClassName` ne seront définies que si l'URL courante est _exactement_ égale à la valeur de la prop `to`. Mais parfois, vous souhaitez rendre le `<Link>` avec le style des éléments actifs même si son lien ne correspond que partiellement à l'URL courante. Par exemple:

- Vous pouvez souhaiter que `/blog/hello-world` corresponde à `<Link to="/blog">`
- Ou que `/gatsby-link/#passing-state-through-link-and-navigate` déclenche le rendu "actif" de `<Link to="/gatsby-link">`

Dans de tels cas, ajoutez simplement la prop `partiallyActive` à votre composant `<Link>` et le style sera appliqué même si la correspondance "URL - valeur de la prop `to`" n'est que partielle:

```jsx
import React from "react"
import { Link } from "gatsby"

const Header = <>
  <Link
    to="/articles/"
    activeStyle={{ color: "red" }}
    {/* highlight-next-line */}
    partiallyActive={true}
  >
    Articles
  </Link>
</>;
```

_**Remarque:** Disponible depuis Gatsby V2.1.31. Si vous rencontrez des problèmes, vérifiez votre version de Gatsby et/ou mettez-la à jour._

### Passer l'état en tant que props à la page cible

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-include-information-about-state-in-navigation-with-gatsby-s-link-component"
  lessonTitle="Inclure les données d'état lors de la navigation en utilisant le composant Link de Gatsby"
/>

Quelquefois vous souhaitez passer des données de la page source vers la page cible. Vous pouvez le faire en passant une prop `state` au composant `Link` ou via un appel à la fonction `navigate`. La page de destination aura alors accès à une prop `location` ayant, entre autres, un attribut `state` qui contiendra les données que vous avez fait passer.

```jsx
const PhotoFeedItem = ({ id }) => (
  <div>
    {/* (on omet la liste des items par souci de concision) */}
    <Link
      to={`/photos/${id}`}
      {/* highlight-next-line */}
      state={{ fromFeed: true }}
    >
      Afficher la Photo
    </Link>
  </div>
)

// highlight-start
const Photo = ({ location, photoId }) => {
  if (location.state.fromFeed) {
    // highlight-end
    return <FromFeedPhoto id={photoId} />
  } else {
    return <Photo id={photoId} />
  }
}
```

### Remplacer l'historique en modifiant le comportement du bouton "retour" _(NdT: bouton 'back')_

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-replace-navigation-history-items-with-gatsby-s-link-component"
  lessonTitle="Remplacer des élémentes de l'historique de navigation en utilisant le composant Link de Gatsby"
/>

Dans certains cas, il peut être utile de modifier le comportement du bouton "retour". Par exemple, si vous créez:
- une page sur laquelle l'utilisateur choisit quelque chose, 
- l'utilisateur verra alors s'afficher une page "En êtes-vous sûr?" (pour s'assurer que c'est effectivement ce qu'il souhaite), 
- et enfin l'utilisateur verra une page de confirmation finale,

il peut être intéressant d'omettre la page "En êtes-vous sûr?" lorsque l'utilisateur choisit de cliquer sur le bouton "retour" à la dernière étape.

Dans de tels cas, utilisez la prop `replace` pour remplacer dans l'historique l'URL actuelle par l'URL cible du `Link`.

```jsx
import React from "react"
import { Link } from "gatsby"

const AreYouSureLink = () => (
  <Link
    to="/confirmation/"
    {/* highlight-next-line */}
    replace
  >
    Oui, j'en suis sûr
  </Link>
)
```

## Comment utiliser la fonction utilitaire `navigate`

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-navigate-to-a-new-page-programmatically-in-gatsby"
  lessonTitle="Naviguer vers une nouvelle page de manière programmatique dans Gatsby"
/>

Parfois vous avez besoin de naviguer vers des pages de façon programmatique, par exemple, pendant l'envoi de formulaires _(NdT: form submission)_. Dans de tels cas, `Link` ne fonctionnera pas.

_**Remarque:** `navigate` portait auparavant le nom de `navigateTo`. `navigateTo` est obsolète dans Gatsby v2 et sera supprimé dans la prochaine mise à jour majeure._

A sa place, Gatsby exporte une fonction utilitaire `navigate` qui accepte deux arguments, `to` et `options`.

| Argument          | Requis | Description                                                                                     |
| ----------------- | -------- | ----------------------------------------------------------------------------------------------- |
| `to`              | oui      | La page cible, vers laquelle on navigue (par exemple, `/blog/`).                                                        |
| `options.state`   | non       | Un objet. Les valeurs passés ici seront disponibles dans l'attribut `location.state` des props de la page cible. |
| `options.replace` | non       | Un booléen. Si vrai, l'URL currente dans l'historique est remplacée.                                  |

Par défaut, `navigate` fonctionne de la même manière qu'un composant cliqué `Link`.

```jsx
import React from "react"
import { navigate } from "gatsby" // highlight-line

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: faire quelque chose avec les valeurs saisies
      // highlight-next-line
      navigate("/form-submitted/")
    }}
  >
    {/* (on omet ici les formulaires de saisie par souci de concision) */}
  </form>
)
```

### Ajouter l'état à la navigation programmatique

Pour transmettre des informations d'état, utilisez l'argument `options` avec l'attribut `state` contenant l'état désiré.

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // L'implementation de cette fonction est laissée en exercice pour le lecteur.
      const formValues = getFormValues()

      navigate(
        "/form-submitted/",
        // highlight-start
        {
          state: { formValues },
        }
        // highlight-end
      )
    }}
  >
    {/* (on omet ici les formulaires de saisie par souci de concision) */}
  </form>
)
```

Ainsi à partir de la page cible, vous pouvez accéder à la valeur `location` comme nous l'avons vu dans [Passer l'état en tant que props à la page cible](#pass-state-as-props-to-the-linked-page).

### Remplacer l'historique lors de la navigation programmatique

Si au lieu d'ajouter une nouvelle entrée dans l'historique, vous souhaitez remplacer l'entrée courante, ajoutez l'attribut `replace`, égal à `true`, à l'argument `options` de la fonction `navigate`.

```jsx
import React from "react"
import { navigate } from "gatsby"

const Form = () => (
  <form
    onSubmit={event => {
      event.preventDefault()

      // TODO: faire quelque chose avec les valeurs saisies
      navigate(
        "/form-submitted/",
        // highlight-next-line
        { replace: true }
      )
    }}
  >
    {/* (on omet ici les formulaires de saisie par souci de concision) */}
  </form>
)
```

## Ajouter un préfixe aux chemins d'accès en utilisant `withPrefix`

Il n'est pas rare d'héberger des sites dans des sous-répertoires d'un autre site. Gatsby vous laisse la possibilité de [définir un préfixe de chemin d'accès](/docs/path-prefix/). _(NdT: path prefix)_ pour votre site. De cette manière, le composant `<Link>` de Gatsby pourra reconstruire automatiquement l'URL correcte à la fois dans les environnements de production et de développement.

Pour des chemins d'accès construits manuellement, il existe une fonction utilitaire, `withPrefix`, qui ajoute un préfixe au début des chemins dans l'environnement de production (mais pas dans l'environnement de développement où les chemins n'ont pas besoin d'être modifiés).

```jsx
import { withPrefix } from "gatsby"

const IndexLayout = ({ children, location }) => {
  const isHomepage = location.pathname === withPrefix("/")

  return (
    <div>
      <h1>Bienvenue {isHomepage ? "à la maison" : "à bord"}!</h1>
      {children}
    </div>
  )
}
```

## Rappel: utilisez `<Link>` uniquement pour des liens internes!

Ce composant est destiné _uniquement_ à définir des liens vers des pages gérées par Gatsby. Pour créer des liens vers d'autres noms de domaine ou des pages du même domaine, mais qui ne sont pas gérées par le site Gatsby courant, utilisez la balise traditionnelle `<a>`.

Parfois vous ne saurez pas à l'avance si un lien sera interne ou non, par exemple lorsque les données proviennent d'un CMS.
Dans ce cas, il peut être intéressant de créer un composant qui examine le lien et choisit de l'afficher soit sous la forme d'un `<Link>` Gatsby, soit via une balise traditionnelle `<a>`, suivant ce qui est le plus approprié.

Puisque savoir si un lien est interne ou non à un site dépend du site en question, vous devrez probablement adapter la logique suivante à votre cas particulier, mais ce qui suit peut tout de même constituer un bon point de départ:

```jsx
import { Link as GatsbyLink } from "gatsby"

// Puisque les éléments <a> du DOM ne peuvent pas recevoir des attributs activeClassName
// et partiallyActive, on déstructure la prop ici et
// on ne la passe qu'au GatsbyLink
const Link = ({ children, to, activeClassName, partiallyActive, ...other }) => {
  // Adaptez le test qui suit à votre environnement.
  // Cet exemple suppose que tous les liens internes (pris en charge par Gatsby)
  // commenceront par exactement un slash, et que toutes les autres adresses sont externes.
  const internal = /^\/(?!\/)/.test(to)

  // Utiliser un Gatsby Link pour les liens internes et <a> pour tous les autres
  if (internal) {
    return (
      <GatsbyLink
        to={to}
        activeClassName={activeClassName}
        partiallyActive={partiallyActive}
        {...other}
      >
        {children}
      </GatsbyLink>
    )
  }
  return (
    <a href={to} {...other}>
      {children}
    </a>
  )
}

export default Link
```

### Téléchargement de fichiers

De la même manière, vous pouvez vérifier les liens de téléchargements:

```jsx
  const file = /\.[0-9a-z]+$/i.test(to)

  ...

  if (internal) {
    if (file) {
        return (
          <a href={to} {...other}>
            {children}
          </a>
      )
    }
    return (
      <GatsbyLink to={to} {...other}>
        {children}
      </GatsbyLink>
    )
  }
```

## Conseils pour la navigation programmatique dans l'application _(NdT: in-app navigation)_

Ni `<Link>`, ni `navigate` de Gatsby ne peuvent être utilisés pour la navigation à l'intérieur d'une même page, pour liens contenant un hash ou une chaîne de requête _(NdT: query parameter)_. Si vous souhaitez implementer un tel comportement, vous devrez:
- soit utiliser une balise d'ancrage _(NdT: anchor tag)_
- soit importer le package `@reach/router` (déjà utilisé par Gatsby) pour pouvoir faire appel à sa fonction `navigate`, comme ci-dessous:

```jsx
import { navigate } from '@reach/router';

...

onClick = () => {
  navigate('#some-link');
  // Ou bien
  navigate('?foo=bar');
}
```

## Le rendu de pages périmées côté client _(NdT: stale client-side pages)_

Le composant `<Link>` de Gatsby ne va chercher qu'une seule fois les ressources de chaque page. Les pages sont alors comme "gelées dans le temps" et leurs mises à jour ultérieures ne seront pas reflétées dans le navigateur Web. Ceci a comme effet indésirable, le fait que différents utilisateurs peuvent avoir des aperçus différents d'un même contenu.

Afin de remédier à ces problèmes de rafraîchissement, lors du chargement d'une nouvelle page, Gatsby va chercher un fichier supplémentaire `app-data.json`. Ce fichier contient un hash généré lorsque le site est compilé _(NdT: built)_; si quoique ce soit change dans le dossier `src`, ce hash sera modifié. Pendant le chargement de page, si Gatsby remarque que le hash stocké dans le fichier `app-data.json` est différent du hash initialement obtenu lors du tout premier chargement du site, le navigateur Web naviguera vers la page en modifiant `window.location`. Ainsi le browser enverra une requête pour obtenir une nouvelle page, affichera un contenu frais et toutes les ressources mises en cache seront perdues.

Cependant, si la page a déjà été chargée précédemment, on ne redemandera `app-data.json`. Dans ce cas, on n'effectue pas de comparaison de hashs et le contenu qui a été chargé la première fois sera réutilisé.

> **Remarque:** Tout état sera perdu lors d'un changement de `window.location`. Cela peut avoir des effets indésirables si l'on se fie à des mécanismes de gestion d'état tels que le stockage d'état dans [wrapPageElement](/docs/browser-apis/#wrapPageElement) ou à des libraries de gestion d'état comme Redux.

## Ressources additionnelles

- [Tutoriel portant sur l'authentification pour le routage effectué côté client uniquement _(NdT: client-only routes)_](/tutorial/authentication-tutorial/)
- [Routage: Obtenir les données de localisation _(NdT: location data)_ à partir de Props](/docs/location-data-from-props/)
- [`gatsby-plugin-catch-links`](https://www.gatsbyjs.org/packages/gatsby-plugin-catch-links/) pour intercepter automatiquement les liens locaux dans des fichiers Markdown et offrir un comportement similaire à `gatsby-link`
