---
title: Spécification API
---

Les API de Gatsby sont conçues conceptuellement dans une certaine mesure d'après React.js pour améliorer la cohérence entre les deux systèmes.

Les deux principales priorités de l'API sont a) permettre un écosystème de plugins large et robuste et b) en plus de cela, un écosystème de thèmes large et robuste.

## Conditions préalables

Si vous ne connaissez pas le cycle de vie de Gatsby, consultez la présentation [Cycle de vie de l'API Gatsby](/docs/gatsby-lifecycle-apis/).

## Plugins

Les plugins peuvent étendre Gatsby de plusieurs manières:

- Recherche de données (par exemple à partir du système de fichiers ou d'une API ou d'une base de données)
- Transformer des données d'un type à un autre (par exemple, un fichier de démarque en HTML)
- Création de pages (par exemple, un répertoire de fichiers markdown qui est intégralement transformé en pages avec des URL dérivées de leurs noms de fichiers).
- Modification de la configuration du webpack (par exemple pour les options de style, ajout de la prise en charge d'autres langages de compilation vers js)
- Ajouter des éléments au HTML rendu (par exemple, des balises meta, des extraits de code JS analytiques comme Google Analytics)
- Rédaction d'éléments pour créer un répertoire en fonction des données du site (par exemple, technicien de service, plan du site, flux RSS)

Un seul plugin peut utiliser plusieurs API pour atteindre son objectif. Par exemple. le plugin pour la bibliothèque CSS-in-JS [Charme](/packages/gatsby-plugin-glamor/):

1.  modifie la configuration du webpack pour ajouter son plugin
2.  ajoute un plugin Babel pour remplacer le createElement par défaut de React
3.  modifie le rendu du serveur pour extraire le CSS critique pour chaque page rendue et intégrer le CSS dans le `<head>` de cette page HTML.

Les plugins peuvent également dépendre d'autres plugins. [Le plugin Sharp](/packages/gatsby-plugin-sharp/) expose un certain nombre d'API de haut niveau pour transformer des images dont d'autres plugins dépendent. [gatsby-transformer-remarque](/packages/gatsby-transformer-remark/) effectue une transformation basique markdown-> html mais expose une API permettant à d'autres plugins d'intervenir dans le processus de conversion, par ex. [gatsby-remarque-prismjs](/packages/gatsby-remark-prismjs/) ce qui ajoute une coloration syntaxique aux blocs de code.

Les plugins Transformer sont découplés des plugins source. Les plugins Transformer examinent le type de média des nouveaux nœuds créés par les plugins source pour décider s'ils peuvent le transformer ou non. Ce qui signifie qu'un plugin transformer markdown peut transformer le markdown de n'importe quelle source sans aucune autre configuration, par exemple à partir d'un fichier, d'un commentaire de code ou d'un service externe comme Trello qui prend en charge le markdown dans certains de ses champs de données.

Voir [la liste complète des plugins (officiels uniquement pour le moment - ajout de la prise en charge des plugins communautaires plus tard)](/docs/plugins/).

## API

### Concepts

- _Page_ - une page de site avec un chemin, un composant de modèle et une requête GraphQL facultative.
- _Page Component_ - Composant React.js qui rend une page et peut éventuellement spécifier une requête GraphQL
- _Component extensions_ — extensions qui peuvent être résolues en tant que composants. `.js` et `.jsx` sont pris en charge par core. Mais les plugins peuvent ajouter la prise en charge d'autres langages de compilation en js.
- _Dependency_ — Gatsby suit automatiquement les dépendances entre différents objets, par ex. une page peut dépendre de certains nœuds. Cela permet le rechargement à chaud, la mise en cache, les reconstructions incrémentielles, etc.
- _Node_ — un objet de données
- _Node Field_ — un champ ajouté par un plugin à un nœud qu'il ne contrôle pas
- _Node Link_ — une connexion entre les nœuds qui est convertie en relations GraphQL. Peut être créé de différentes manières et automatiquement déduit. Les liens parent / enfant des nœuds et leurs nœuds dérivés transformés sont des liens de première classe.

_D'autres définitions et termes sont définis dans le [Glossaire](/docs/glossary/)_

### Les opérateurs

- _Create_ — faire une nouvelle chose
- _Get_ — obtenir une chose existante
- _Delete_ — supprimer un élément existant
- _Replace_ — remplacer une chose existante
- _Set_ — fusionner dans une chose existante

### API d'extension

Gatsby a plusieurs processus. Le plus important est le processus de "bootstrap". Il comporte plusieurs sous-processus. Une partie délicate de leur conception est qu'ils s'exécutent une fois pendant le bootstrap initial, mais restent également en vie pendant le développement pour continuer à répondre aux changements. C'est ce qui pousse au rechargement à chaud que toutes les données Gatsby sont "vivantes" et réagissent aux changements de l'environnement.

Le processus de bootstrap est le suivant:

charger la configuration du site -> charger les plugins -> nœuds source -> transformer les nœuds -> créer un schéma graphql -> créer des pages -> compiler des requêtes de composants -> exécuter des requêtes -> fin

Une fois le bootstrap initial terminé, un `webpack-dev-server` et un serveur express sont démarrés pour servir les fichiers pour le workflow de développement avec des mises à jour en direct. Pour une version de production, Gatsby ignore le serveur de développement et construit à la place le CSS, puis le JavaScript, puis le HTML statique avec webpack.

Au cours de ces processus, il existe différents points d'extension où les plugins peuvent intervenir. Tous les processus majeurs ont un `onPre` et `onPost` par exemple `onPreBootstrap` et `onPostBootstrap` ou `onPreBuild` ou `onPostBuild`. Pendant le bootstrap, les plugins peuvent répondre à différentes étapes aux API telles que `onCreatePages`, `onCreateBabelConfig`, et `onSourceNodes`.

A chaque point d'extension, Gatsby identifie les plugins qui implémentent l'API et les appelle en série en suivant leur ordre dans le fichier de configuration `gatsby-config.js`.

En plus des API d'extension dans un nœud, les plugins peuvent également implémenter des API d'extension dans le processus de rendu du serveur et le navigateur, par exemple `onClientEntry` ou `onRouteUpdate`.

Les trois principales inspirations pour cette API et cette spécification sont l'API de React.js spécifiquement [Mail de @leebyron sur l'API React](https://gist.github.com/vjeux/f2b015d230cc1ab18ed1df30550495ed), ce discours ["How to Design a Good API and Why it Matters" par Joshua Bloch](https://www.youtube.com/watch?v=heh4OeB9A-c&app=desktop) qui a conçu de nombreuses parties de Java, et [Hapi.js](https://hapijs.com/api)' conception de plugin.
