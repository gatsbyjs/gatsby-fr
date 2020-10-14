---
title: Fichiers API
---

Gatsby utilise 4 fichiers à la racine de votre projet pour configurer votre site et contrôler son comportement. Tous ces fichiers sont optionnels.

- [gatsby-config.js](/docs/api-files-gatsby-config) - Active les plugins, définit les données communes du site et contient d'autres configurations du site qui s'intègrent à la couche de données GraphQL de Gatsby.
- [gatsby-browser.js](/docs/api-files-gatsby-browser) - Vous permet de contrôler le comportement de Gatsby dans le navigateur. Par exemple, répondre à un utilisateur qui change de route, ou appeler une fonction lorsque l'utilisateur ouvre une page pour la première fois.
- [gatsby-node.js](/docs/api-files-gatsby-node) - Vous permet de répondre aux événements dans le cycle de développement de Gatsby. Par exemple, ajouter des pages dynamiquement, modifier les nœuds GraphQL au fur et à mesure de leur création, ou effectuer une action après la fin de la construction.
- [gatsby-ssr.js](/docs/api-files-gatsby-ssr) - Expose le processus de rendu de Gatsby côté serveur afin que vous puissiez contrôler la façon dont il construit vos pages HTML.
