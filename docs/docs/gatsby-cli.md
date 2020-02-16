---
title: Commandes (Gatsby CLI)
tableOfContentsDepth: 2
---

L'outil de ligne de commande Gatsby (CLI) est le principal point d'entrée pour démarrer une application Gatsby et pour utiliser des fonctionnalités telles que le lancement d'un serveur de développement et la création de votre application Gatsby pour le déploiement.

_Nous fournissons une documentation similaire disponible avec le gatsby-cli [LISEZ-MOI](https://github.com/gatsbyjs/gatsby/blob/master/packages/gatsby-cli/README.md), et notre [cheat sheet](/docs/cheat-sheet/) dispose de toutes les principales commandes CLI prêtes à être exécutées._

## Comment utiliser gatsby-cli

Le Gatsby CLI (`gatsby-cli`) est un exécutable qui peut être utilisé globalement. Le Gatsby CLI est disponible via [npm](https://www.npmjs.com/) et devrait être installée globalement en exécutant `npm install -g gatsby-cli` pour l'utiliser localement.

Exécutez `gatsby --help` pour une aide complète.

Vous pouvez aussi utiliser la variante de script `package.json` de ces commandes, typiquement exposées _pour vous_ avec la plupart des [starters](/docs/starters/). Par exemple, si vous voulez rendre la commande[`gatsby develop`](#develop) disponible dans votre application, ouvrez `package.json` et ajoutez un script comme ceci :

```json:title=package.json
{
  "scripts": {
    "develop": "gatsby develop"
  }
}
```

## Commandes API

### `new`


```shell
gatsby new [<nom-de-site> [<url-de-départ>]]

```

#### Arguments

| Argument    | Description                                                                                                                                                                                                     |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| nom-de-site   | Le nom de votre site Gatsby, qui est également utilisé pour créer un répertoire de projet.                                                                                                                                        |
| url-de-départ | Une URL de démarrage Gatsby ou un chemin de fichier local. Par défaut :[gatsby-starter-default](https://github.com/gatsbyjs/gatsby-starter-default); voir les docs [Gatsby starters](/docs/gatsby-starters/) pour plus d'informations. |

> Note: Le `nom-de-site` ne doit être composé que de lettres et de chiffres. Si vous spécifiez un `.`, `./` ou un `<espace>` dans le nom, `gatsby new` provoquera une erreur.

#### Exemples

- Créez un site Gatsby nommé `my-awesome-site` en utilisant le starter par défaut:

```shell
gatsby new my-awesome-site
```

- Créez un site Gatsby nommé `my-awesome-blog-site`, en utilisant [gatsby-starter-blog](https://www.gatsbyjs.org/starters/gatsbyjs/gatsby-starter-blog/):

```shell
gatsby new my-awesome-blog-site https://github.com/gatsbyjs/gatsby-starter-blog
```

- Si vous omettez les deux arguments, le CLI lancera un shell interactif demandant ces entrées:

```shell
gatsby new
? What is your project called? › my-gatsby-project
? What starter would you like to use? › - Use arrow-keys. Return to submit.
❯  gatsby-starter-default
   gatsby-starter-hello-world
   gatsby-starter-blog
   (Use a different starter)
```

Voir les [docs de Gatsby starters](https://www.gatsbyjs.org/docs/gatsby-starters/) pour plus de détails.

### `develop`

Une fois que vous avez installé un site Gatsby, allez dans le répertoire racine de votre projet et démarrez le serveur de développement:

`gatsby develop`

#### Options

|     Option      | Description                                     |
| :-------------: | ----------------------------------------------- |
| `-H`, `--host`  | Définit hôte. Par défaut à localhost            |
| `-p`, `--port`  | Définit port. Par défaut à 8000                 |
| `-o`, `--open`  | Ouvre le site dans votre navigateur (par défaut)|
|                 | pour vous                                       |
| `-S`, `--https` | Utilise HTTPS                                   |


Suivez le [Guide HTTPS local](/docs/local-https/)
pour savoir comment configurer un serveur de développement HTTPS avec Gatsby.

#### Prévisualisation des modifications sur d'autres appareils

Vous pouvez utiliser la commande Gatsby develop avec l'option host pour accéder à votre environnement dev sur d'autres périphériques du même réseau, lancez:

```shell
gatsby develop -H 0.0.0.0
```

Ensuite, le terminal enregistrera les informations comme d'habitude, mais inclura en plus une URL vers laquelle vous pouvez naviguer depuis un client sur le même réseau pour voir comment le site est rendu.

```shell
Vous pouvez maintenant voir gatsbyjs.org dans le navigateur.
⠀
  Local:            http://0.0.0.0:8000/
  Sur votre réseau:  http://192.168.0.212:8000/ // highlight-line
```

**Note** : Pour accéder à Gatsby depuis votre machine locale, utilisez soit `http://localhost:8000`, soit l'URL "Sur votre réseau".

### `build`

A la racine d'un site Gatsby, compilez votre application et préparez son déploiement:

`gatsby build`

#### Options

|            Option            | Description                                                                                               |
| :--------------------------: | --------------------------------------------------------------------------------------------------------- |
|       `--prefix-paths`       | Construit un site avec des chemins de liens préfixés (définissez pathPrefix dans votre configuration) |
|        `--no-uglify`         | Crée un site sans avoir à dissocier les paquets JS (pour le débogage)                                                   |
| `--open-tracing-config-file` | Fichier de configuration du traceur (compatible OpenTracing). 
Voir [Suivi des performances du traceur](/docs/performance-tracing/) |
| `--no-color`, `--no-colors`  | Désactive la sortie du terminal en couleur                                                                         |

En plus de ces options de build, il y a quelques options optionnelles [variables d'environnement de build] (/docs/environment-variables/#build-variables) pour des configurations plus avancées qui peuvent ajuster le fonctionnement d'un build. Par exemple, le réglage de `CI=true` comme variable d'environnement personnalisera la sortie pour les [terminaux bêtes] (https://en.wikipedia.org/wiki/Computer_terminal#Dumb_terminals).

### `serve`

A la racine d'un site Gatsby, servez le build de production de votre site pour les tests:

`gatsby serve`

#### Options

|      Option      | Description                                                                              |
| :--------------: | ---------------------------------------------------------------------------------------- |
|  `-H`, `--host`  | Définit hôte. Par défaut à localhost                                                     |
|  `-p`, `--port`  | Définit port. Par défaut à 9000                                                          |
|  `-o`, `--open`  | Ouvre le site dans votre navigateur (par défaut) pour vous                               |
| `--prefix-paths` | Servi le site avec les chemins de liens préfixés (si construit avec pathPrefix dans votre|
|                  | gatsby-config.js) |

### `info`

A la racine d'un site Gatsby, obtenez des informations utiles sur l'environnement qui seront nécessaires pour signaler un bug:

`gatsby info`

#### Options

|       Option        | Description                                             |
| :-----------------: | ------------------------------------------------------- |
| `-C`, `--clipboard` | Copie automatiquement les informations d'environnement  |
|                     |dans le presse-papiers |

### `clean`

A la racine d'un site Gatsby, effacez le cache (dossier `.cache`) et les répertoires publics:

`gatsby clean`

Ceci est utile en dernier recours lorsque votre projet local semble avoir des problèmes ou que le contenu ne semble pas rafraîchir. Les problèmes que cela peut résoudre comprennent généralement:

- Les données périmées, par exemple ce fichier/ressource/etc. n'apparait pas.
- Erreur de GraphQL, par exemple cette ressource GraphQL doit être présente mais n'est pas
- Problèmes de dépendance, par exemple, version invalide, erreurs cryptographiques dans la console, etc.
- Problèmes de plugin, par exemple, le développement d'un plugin local et les changements ne semblent pas prendre effet.

### `plugin`

Exécuter des commandes se référant aux plugins Gatsby.

#### `docs`

`gatsby plugin docs`

Vous dirige vers la documentation sur l'utilisation et la création de plugins.

### Repl

Obtenez un REPL Node.js (shell interactif) avec le contexte de votre environnement Gatsby:

`gatsby repl`

Gatsby vous invitera à taper des commandes et à explorer. Quand il montre ceci: `gatsby >`

Vous pouvez taper une commande comme celle-ci:

`babelrc`

`components`

`dataPaths`

`getNodes()`

`nodes`

`pages`

`schema`

`siteConfig`

`staticQueries`

Combinées avec [l'explorateur GraphQL] (/docs/introducing-graphiql/), ces commandes REPL peuvent être très utiles pour comprendre les données de votre site Gatsby.

Pour plus d'informations, consultez la [documentation Gatsby REPL] (/docs/gatsby-repl/).

### Désactivation de la sortie colorée

En plus de l'option explicite `--no-color`, la CLI respecte la présence de la variable d'environnement `NO_COLOR` (voir [no-color.org](https://no-color.org/)).

## Comment changer de gestionnaire de paquets par défaut sur votre prochain projet ?

Lorsque vous utilisez `gatsby new` la première fois pour créer un nouveau projet, il vous demande de choisir votre gestionnaire de paquets par défaut entre yarn et npm.

```shell
Which package manager would you like to use ? › - Use arrow-keys. Return to submit.
❯  yarn
   npm
```

Une fois que vous avez fait votre choix, la CLI ne vous demandera plus vos préférences même pour de nouveaux projets.

Si vous souhaitez changer ce paramètre pour votre prochain projet vous devez modifier le fichier de configuration qui a été créé automatiquement par la CLI.
Sur votre système, ce fichier est disponible à : `~/.config/gatsby/config.json`

Dans ce fichier vous verrez quelque chose de similaire à ceci.

```json:title=config.json
{
  "cli": {
    "packageManager": "yarn"
  }
}
```

Modifiez la valeur de `packageManager`, enregistrez et vous êtes pret à attaquer votre prochain projet avec `gatsby new`.
