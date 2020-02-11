---
titre:  Démarrage rapide
---

Cette section de démarrage rapide est destinée aux développeurs intermédiaires à avancés. Pour une introduction plus douce à Gatsby, rendez-vous sur la page de [notre tutoriel](/tutorial/) !

## Utiliser Gatsby CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Quick Start with Gatsby: Create, Develop, and Build Gatsby Sites From the Command Line"
/>

**Note** : cette vidéo utilise `npx`, un outil permettant d’exécuter un package npm sans l'installer d'abord. Exécuter la commande `npx gatsby new` est la même chose que d’exécuter `gatsby new` après l'installation de gatsby-cli sur votre ordinateur.

<<<<<<< HEAD
### Installer Gatsby CLI
=======
### Install the Gatsby CLI
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

```shell
npm install -g gatsby-cli
```

<<<<<<< HEAD
### Créer un nouveau site
=======
### Create a new site
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

```shell
gatsby new gatsby-site
```

<<<<<<< HEAD
### Se déplacer vers le dossier du site
=======
### Change directories into site folder
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

```shell
cd gatsby-site
```

<<<<<<< HEAD
### Lancer son serveur de développement
=======
### Start development server
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

```shell
gatsby develop
```

<<<<<<< HEAD
Gatsby lance un environnement de développement disposant du hot-reloading et accessible par défaut à `localhost:8000`.
=======
Gatsby will start a hot-reloading development environment accessible by default at `http://localhost:8000`.
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

Essayez d'éditer les pages JavaScript disponible dans le dossier `src/pages`. Les modifications enregistrées seront rechargées en direct dans le navigateur (hot-reloading).

<<<<<<< HEAD
### Créer un build pour mise en production
=======
### Create a production build
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

```shell
gatsby build
```

Gatsby effectue un build de production optimisé pour votre site, générant le HTML static et des bundles du JavaScript pour chaque route.

<<<<<<< HEAD
### Servir le build de production localement
=======
### Serve the production build locally
>>>>>>> fd3df38d5351bfbf1bf86cb9e0c8cc80dc9ba2a7

```shell
gatsby serve
```

Gatsby lance un serveur HTML local pour tester votre site construit. N'oubliez pas de construire votre site en utilisant `gatsby build` avant d'utiliser cette commande.

### Accéder à la documentation pour les commandes CLI

Pour voir la documentation détaillée des commandes CLI, exécutez `gatsby --help` dans le terminal.

Pour des commandes spécifiques, exécutez `gatsby COMMAND_NAME --help` par exemple `gatsby new --help`.

Pour plus d'informations sur Gatsby CLI, visitez la section [Référence CLI](/docs/gatsby-cli/) de la documentation.
