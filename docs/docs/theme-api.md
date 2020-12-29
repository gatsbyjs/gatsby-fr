---
title: Référence de l'API Themes
---

## Coeur Gatsby APIs

Les thèmes sont des sites Gatsby fournis sous forme de plug-ins, vous avez donc accès à toutes les API de Gatsby pour modifier les paramètres de configuration et les fonctionnalités par défaut.

- [Gatsby Config](https://www.gatsbyjs.org/docs/gatsby-config/)
- [Actions](https://www.gatsbyjs.org/docs/actions/)
- [Node Interface](https://www.gatsbyjs.org/docs/node-interface/)
- ... [et plus](https://www.gatsbyjs.org/docs/api-specification/)

Si vous êtes nouveau sur Gatsby, vous pouvez commencer en suivant les guides de création d'un site. Le convertir en thème sera simple plus tard, car les thèmes sont des sites Gatsby pré-emballés.

## Configuration

Les plugins peuvent désormais inclure un `gatsby-config` en plus de l'autre `gatsby-*` Fichiers. Nous faisons généralement référence aux plugins qui incluent un `gatsby-config.js` comme thème (plus à ce sujet dans [composition du thème](#theme-composition)). Un typique `gatsby-config.js` dans le site d'un utilisateur qui utilise votre thème pourrait ressembler à ceci. Cet exemple passe en deux options à `gatsby-theme-name`: `postsPath` et `colors`.

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: "gatsby-theme-name",
      options: {
        postsPath: "/blog",
        colors: {
          primary: "tomato",
        },
      },
    },
  ],
}
```

Vous pouvez accéder aux options qui sont transmises à votre thème dans votre thème `gatsby-config`. Vous pouvez utiliser des options pour rendre le sourçage du système de fichiers configurable, accepter différents éléments du menu de navigation, modifier les couleurs de marque par défaut et tout ce que vous souhaitez rendre configurable.

Pour profiter des options transmises lors de la configuration de votre thème dans le site d'un utilisateur, renvoyez une fonction dans votre thème `gatsby-config.js`. L'argument que la fonction reçoit sont les options transmises par l'utilisateur.

```js:title=gatsby-config.js
module.exports = themeOptions => {
  console.log(themeOptions)
  // logs `postsPath` and `colors`

  return {
    plugins: [
      // ...
    ],
  }
}
```

Lors de l'utilisation de l'exportation d'objets habituelle (`module.exports = {}`) dans votre thème signifie que vous pouvez exécuter le thème de manière autonome comme son propre site, lorsque vous utilisez une fonction de votre thème pour accepter des options, vous devrez exécuter le thème dans le cadre d'un exemple de site. Voyez comment le [starter de création de thème](https://github.com/gatsbyjs/gatsby-starter-theme-workspace) gère cela à l'aide d'espaces de travail Yarn.

### Accéder aux options ailleurs

Notez que les thèmes étant des plugins, vous pouvez également accéder aux options de l'une des méthodes de cycle de vie auxquelles vous êtes habitué. Par exemple, dans votre thème `gatsby-node.js` vous pouvez accéder aux options comme deuxième argument de `createPages`:

```js:title=gatsby-node.js
exports.createPages = async ({ graphql, actions }, themeOptions) => {
  console.log(themeOptions)
}
```

## Ombrage

Étant donné que les thèmes sont généralement déployés en tant que packages npm que d'autres personnes utilisent dans leurs sites, vous avez besoin d'un moyen de modifier certains fichiers, tels que les composants React, sans apporter de modifications au code source du thème. C'est appelé _Shadowing_.

L'observation est une API basée sur un système de fichiers qui nous permet de remplacer un fichier par un autre au moment de la construction. Par exemple, si vous aviez un thème avec un `Header` composant que vous pourriez remplacer `Header`avec le vôtre en créant un nouveau fichier et en le plaçant au bon emplacement pour que l'observation le trouve.

### Primordial

En regardant de plus près `Header` exemple, disons que vous avez un thème appelé `gatsby-theme-amazing`. Ce thème utilise un `Header` composant pour rendre la navigation et d'autres éléments divers. Le chemin d'accès au composant à partir de la racine du package npm est `gatsby-theme-amazing/src/components/header.js`.

Vous voudrez peut-être le `Header` composant pour faire quelque chose de différent (peut-être changer les couleurs, peut-être ajouter des éléments de navigation supplémentaires, vraiment tout ce à quoi vous pouvez penser). Pour ce faire, vous créez un fichier dans votre site à l'adresse `src/gatsby-theme-amazing/components/header.js`.Vous pouvez maintenant exporter n'importe quel composant React que vous souhaitez à partir de ce fichier et Gatsby l'utilisera à la place du composant du thème.

> 💡Remarque: vous pouvez masquer les composants d'autres thèmes en utilisant la même méthode. En savoir plus sur les applications avancées dans [ombre latente](https://johno.com/latent-component-shadowing).

### Extending

Dans la dernière section, nous avons parlé de remplacer complètement un composant par un autre. Que faire si vous souhaitez apporter une modification plus petite qui ne nécessite pas de copier / coller l'intégralité du composant de thème dans le vôtre? Vous pouvez profiter de la possibilité d'étendre les composants.

Prenant le `Header` exemple d'avant, lorsque vous écrivez votre fichier d'observation à `src/gatsby-theme-amazing/components/header.js`, vous pouvez importer le composant d'origine et le réexporter en tant que tel, en ajoutant votre propre accessoire remplacé au composant.

```js
import Header from "gatsby-theme-amazing/src/components/header"

// these props are the same as the original component would get
export default props => <Header {...props} myProp="true" />
```

Cette approche signifie que lorsque vous mettez à jour votre thème plus tard, vous pouvez également profiter de toutes les mises à jour du `Header` composant parce que vous ne l'avez pas entièrement remplacé, il suffit de le modifier.

### Quel chemin doit être utilisé pour masquer un fichier?

Jusqu'à ce que Gatsby dispose d'outils pour gérer automatiquement l'observation, vous devrez localiser manuellement les chemins dans un thème et créer les chemins d'ombrage corrects dans votre site.

Heureusement, le moyen d'y parvenir ne se fait qu'en quelques étapes. Prendre la `src` répertoire du thème, et déplacez-le au début du chemin, puis écrivez un fichier à cet emplacement de votre site. En regardant en arrière sur le `Header` exemple, voici le chemin d'accès au composant dans votre thème:

```text
gatsby-theme-amazing/src/components/header.js
```

et voici le chemin où vous voudriez l'observer dans votre site:

```text
<your-site>/src/gatsby-theme-amazing/components/header.js
```

L'observation ne fonctionne que sur les fichiers importés dans `src` annuaire. Cela est dû au fait que l'observation est construite au-dessus de Webpack, de sorte que le graphique du module doit inclure le fichier shadowable.

Étant donné que vous pouvez utiliser plusieurs thèmes dans un site donné, il existe de nombreux emplacements potentiels pour masquer un fichier donné (un pour chaque thème et un pour le site de l'utilisateur). Dans le cas où plusieurs thèmes tentent d'observer `gatsby-theme-amazing/src/components/header.js`,le dernier thème inclus dans le tableau des plugins l'emportera. Le site lui-même a la plus haute priorité dans l'observation.

## Composition du thème

Les thèmes Gatsby peuvent être composés horizontalement et verticalement. La composition verticale fait référence à la relation classique «parent / enfant». Un thème enfant déclare un thème parent dans le tableau des plugins du thème enfant.

```js:title=gatsby-theme-child/gatsby-config.js
module.exports = {
  plugins: [`gatsby-theme-parent`],
}
```

La composition horizontale est lorsque deux thèmes différents sont utilisés ensemble, tels que `gatsby-theme-blog` et `gatsby-theme-notes`.

```js:title=my-site/gatsby-config.js
module.exports = {
  plugins: [`gatsby-theme-blog`, `gatsby-theme-notes`],
}
```

Les thèmes à leur base sont un algorithme qui fusionne plusieurs `gatsby-config.js` fichiers ensemble dans une seule configuration que votre site peut utiliser pour créer des fichiers. Pour ce faire, vous devez définir comment combiner deux `gatsby-config.js`s ensemble. Avant de pouvoir faire cela, vous devez aplatir les relations parent / enfant en un seul tableau. Cela entraîne le classement final lors de l'examen du fichier d'ombrage à utiliser si plusieurs sont disponibles.

Le premier exemple aboutit à une commande finale de `['gatsby-theme-parent', 'gatsby-theme-child']` (les parents viennent toujours avant leurs enfants afin que les enfants puissent remplacer la fonctionnalité), tandis que le deuxième exemple aboutit à `['gatsby-theme-blog', 'gatsby-theme-notes']`.

Une fois que vous avez l'ordre final des thèmes, vous les fusionnez à l'aide d'une fonction de réduction. [This reduce function](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby/src/utils/merge-gatsby-config.js) spécifie la manière dont chaque clé entre `gatsby-config.js` fusionneront ensemble. Sauf indication contraire ci-dessous, la dernière valeur l'emporte.

- `siteMetadata` et `mapping` les deux fusionnent profondément en utilisant le lodash `merge` fonction. Cela signifie qu'un thème peut définir des valeurs par défaut dans `siteMetadata` et le site peut les remplacer en utilisant la norme `siteMetadata` objet dans `gatsby-config.js`.
- `plugins` sont normalisés pour supprimer les doublons, puis concaténés ensemble.
