---
title: "Recettes: Travailler avec des thèmes"
tableOfContentsDepth: 1
---

Un [thème Gatsby](/docs/themes/what-are-gatsby-themes) synthétise la configuration de Gatsby (fonctionnalité partagée, source de données, conception) dans un paquet installable. Cela signifie que la configuration et les fonctionnalités ne sont pas directement écrites dans votre projet, mais plutôt versionnées, gérées de manière centrale et installées en tant que dépendance. Vous pouvez mettre à jour un thème en toute simplicité, composer des thèmes dans un même ensemble, et même remplacer un thème compatible par un autre.

## Création d'un nouveau site à partir d'un thème

Vous avez trouvé un thème que vous aimeriez utiliser pour votre projet ? Génial ! Vous pouvez le configurer en suivant les étapes ci-dessous.

> Si vous souhaitez consulter d'autres options de thèmes, consultez cette [liste de thèmes](/plugins?=gatsby-theme).

### Prérequis

- Assurez-vous que l'[interface en ligne de commande de Gatsby](/docs/gatsby-cli) est installée.

### Instructions

1. Créer un site gatsby

```shell
gatsby new {your-project-name}
```

2. Changement de répertoire et installation du thème

Dans cet exemple, notre thème est `gatsby-theme-blog`. Vous pouvez remplacer cette référence par le nom de votre thème, quel qu'il soit.

```shell
cd {your-project-name}
npm install gatsby-theme-blog
```

3. Ajouter un thème à `gatsby-config.js`

Suivez les instructions qui se trouvent dans le README du thème que vous utilisez pour déterminer la configuration requise.

```javascript:title=gatsby-config.js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-theme-blog`,
      options: {
        /*
        - basePath defaults to `/`
        - contentPath defaults to `content/posts`
        - assetPath defaults to `content/assets`
        - mdx defaults to `true`
        */
        basePath: `/blog`,
      },
    },
  ],
}
```

4. Lancer `gatsby develop`, le thème devrait maintenant être en cours d'exécution sur votre site. Le thème du blog étant configuré avec le `basePath` vers `"/blog"`, il devrait être accessible à l'adresse `http://localhost:8000/blog`.

> Pour savoir comment personnaliser plus précisément un thème, consultez les chemins d'accès existant dans [la documentation Gatsby-theme-blog](/packages/gatsby-theme-blog/#theme-options).

### Ressources additionnelles

- Pour savoir comment personnaliser plus en détail un thème, consultez la documentation sur le [theme shadowing de Gatsby.](/docs/themes/shadowing/)

- Vous pouvez également [utiliser plusieurs thèmes](/docs/themes/using-multiple-gatsby-themes/) sur un projet.

## Création d'un nouveau site à l'aide d'un thème de départ

La création d'un site basé sur une configuration de démarrage qui configure un thème suit le même processus que la création d'un site basé sur une configuration de démarrage qui **ne configure pas** un thème. Dans cet exemple, vous pouvez utiliser la [configuration de démarrage pour créer un nouveau site qui utilise le thème officiel du blog Gatsby](https://github.com/gatsbyjs/gatsby-starter-blog-theme).

### Prérequis

- Assurez-vous que l'[interface en ligne de commande de Gatsby ](/docs/gatsby-cli) est installée

### Instructions

1. Générer un nouveau site basé sur le thème de départ du blog :

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-blog-theme
```

2. Faites fonctionner votre nouveau site :

```shell
cd {your-project-name}
gatsby develop
```

### Ressources additionnelles

- Apprenez à utiliser un thème Gatsby existant dans le [guide conceptuel abrégé](/docs/themes/using-a-gatsby-theme) ou dans le [tutoriel pas à pas](/tutorial/using-a-theme) plus détaillé.

## Créer un nouveau thème

<EggheadEmbed
  lessonLink="https://egghead.io/lessons/gatsby-use-the-gatsby-theme-workspace-starter-to-begin-building-a-new-theme"
  lessonTitle="Use the Gatsby Theme Workspace Starter to Begin Building a New Theme"
/>

### Prérequis

- L'[interface en ligne de commande de Gatsby ](/docs/gatsby-cli) est installée
- [Yarn](https://yarnpkg.com/lang/en/docs/install/#mac-stable) est installé

### Instructions

1. Générez un nouvel espace de travail thématique en utilisant la [configuration de démarrage du thème espace de travail de Gatsby](https://github.com/gatsbyjs/gatsby-starter-theme-workspace):

```shell
gatsby new {your-project-name} https://github.com/gatsbyjs/gatsby-starter-theme-workspace
```

2. Exécutez le site en exemple dans l'espace de travail :

```shell
yarn workspace example develop
```

### Ressources additionnelles

- Suivez un [guide plus détaillé](/docs/themes/building-themes/) sur l'utilisation de l'espace de travail du thème Gatsby.
- Apprenez à concevoir votre propre thème dans le [cours vidéo sur la création de thèmes Gatsby sur Egghead](https://egghead.io/courses/gatsby-theme-authoring), ou dans le [dossier complémentaire du cours vidéo](/tutorial/building-a-theme).
