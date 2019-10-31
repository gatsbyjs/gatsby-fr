---
title: Démarrage rapide
---

Cette page de démarrage rapide est dédié aux développeurs d'un niveau intermédiaire à avancé. Pour une intro plus facile à Gatsby, [dirigez-vous vers notre tutoriel](/tutorial/)!

## Utiliser le Gatbsy CLI

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-quick-start-with-gatsby-create-develop-and-build-gatsby-sites-from-the-command-line"
  lessonTitle="Démarrage rapide avec Gatsby : Créer, Développer et Builder les sites Gatbsy en ligne de commande"
/>

**Note**: Cette vidéo utilise `npx`, qui est un outil pour éxecuter un package npm sans avoir à l'installer d'abord. Lancer la commande `npx gatsby new` est la même chose que lancer `gatsby new` après avoir installé le gatsby-cli sur votre ordinateur.

### Installer le Gatsby CLI.

```shell
npm install -g gatsby-cli
```

### Créer un nouveau site

```shell
gatsby new gatsby-site
```

### Changer les répertoires dans le dossier du site.

```shell
cd gatsby-site
```

### Démarrer le serveur de développement.

```shell
gatsby develop
```

Gatsby va démarrer un environnement de réchargement à chaud accessible par défaut à l'adresse `localhost:8000`.

Essayez d'éditer les pages JavaScript dans `src/pages`. Sauvegarder un changement va recharger automatiquement votre page dans le navigateur.

### Créer un build de production.

```shell
gatsby build
```

Gatsby va faire un build de votre site optimisé pour la production, en générant du HTML statique et des bundles de code JavaScript par route.

### Démarrer le build de production en local.

```shell
gatsby serve
```

Gatsby démarre serveur HTML local pour tester votre site buildé. Rappelez-vous de builder votre site en utilisant `gatsby build` avant d'utiliser cette commande.

### Accéder à la documentation pour les commandes CLI

Pour voir la documentation détaillée pour les commandes CLI, lancez `gatsby --help` dans le terminal.

Pour des commandes spécifiques, lancez `gatsby COMMAND_NAME --help` par exemple `gatsby new --help`.

Pour plus d'informations sur le Gatbsy CLI, visitez la section [CLI reference](/docs/gatsby-cli/) de la documentation.
