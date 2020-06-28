---
title: Le fichier API gatsby-config.js
---

Le fichier `gatsby-config.js` définit les métadonnées, plugins et autres configurations générales de votre site. Ce fichier doit se trouver à la racine de votre site Gatsby.

Si vous avez créé un site Gatsby avec la commande `gatsby new`, il devrait déjà y avoir un exemple de fichier de configuration dans le répertoire de votre site.

## Installer le fichier de configuration

Le fichier de configuration doit exporter un objet JavaScript. Dans cet objet, vous pouvez définir plusieurs options de configuration différentes.

```javascript:title=gatsby-config.js
module.exports = {
  //objet de configuration
}
```

Un exemple de fichier `gatsby-config.js` pourrait ressembler à ceci :

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Gatsby`,
  },
  plugins: [
    `gatsby-transform-plugin`,
    {
      resolve: `gatsby-plugin-name`,
      options: {
        optionA: true,
        optionB: `Another option`,
      },
    },
  ],
}
```

## Options de configuration

Il existe [de nombreuses options de configuration](/docs/gatsby-config) disponibles, mais les métadonnées les plus courantes définies pour l'ensemble du site et les plugins d'activation.

### Metadata site

L'objet `siteMetadata` peut contenir toutes les données que vous souhaitez partager sur votre site. Un exemple utile est le titre du site. Si vous stockez le titre dans `siteMetadata`, vous pouvez modifier le titre en un seul endroit, et il sera mis à jour dans tout votre site. Pour ajouter des métadonnées, incluez un objet `siteMetadata` dans votre fichier de configuration:

```javascript:title=gatsby-config.js
module.exports = {
  siteMetadata: {
    title: `Gatsby`,
  },
}
```
Vous pouvez ensuite [accéder au titre du site à l'aide de GraphQL](/tutorial/part-four/#your-first-graphql-query) n'importe où sur votre site.

### Plugins

Les plugins ajoutent de nouvelles fonctionnalités à votre site Gatsby. Par exemple, certains plugins récupèrent des données à partir de services hébergés, transforment des formats de données ou redimensionnent des images. La [bibliothèque de plugins Gatsby](/plugins) vous aide à trouver le plugin adapté à vos besoins.

L'installation d'un plugin à l'aide d'un gestionnaire de paquets comme `npm` **ne** l'activera pas sur votre site Gatsby. Pour terminer l'ajout d'un plugin, assurez-vous que votre fichier `gatsby-config.js` possède un tableau` plugins` afin que vous puissiez inclure un espace pour les plugins nécessaires à la construction de votre site:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    //les plugins vont ici
  ],
}
```

Lors de l'ajout de plusieurs plugins, ils doivent être séparés par des virgules dans le tableau `plugins` pour prendre en charge une syntaxe JavaScript valide.

#### Plugins sans options

Si un plugin ne nécessite aucune option, vous pouvez ajouter son nom sous forme de chaîne au tableau `plugins`:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [`gatsby-plugin-name`],
}
```

#### Plugins avec options

De nombreux plugins ont des options facultatives ou obligatoires pour les configurer. Au lieu d'ajouter une chaîne de nom au tableau `plugins`, ajoutez un objet avec son nom et ses options. La plupart des plugins montrent des exemples dans leur fichier ou page `README` dans la [bibliothèque de plugins Gatsby](/plugins).

Voici un exemple montrant comment écrire un objet avec des clés pour `résoudre` le nom du plugin et un objet `options` avec tous les paramètres applicables:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-name`,
      options: {
        optionA: true,
        optionB: `Another option`,
      },
    },
  ],
}
```

#### Plugins mixtes

Vous pouvez ajouter des plugins avec et sans options dans le même tableau. Le fichier de configuration de votre site pourrait ressembler à ceci:

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transform-plugin`,
    {
      resolve: `gatsby-plugin-name`,
      options: {
        optionA: true,
        optionB: `Another option`,
      },
    },
  ],
}
```

## Options de configuration supplémentaires

Il existe plusieurs autres options de configuration disponibles pour `gatsby-config.js`. Vous pouvez trouver une liste de toutes les options dans la page [Gatsby Configuration API](/docs/gatsby-config/).
