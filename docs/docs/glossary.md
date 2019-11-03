---
title: Glossary
disableTableOfContents: true
---

import HorizontalNavList from "../../www/src/components/horizontal-nav-list.js"

Lorsque vous découvrez Gatsby pour la première fois, il peut y avoir beaucoup de mots à apprendre. Ce glossaire a pour but de vous donner un aperçu des termes courants et de leur signification pour les sites Gatsby.

<HorizontalNavList
items={"ABCDEFGHIJKLMNOPQRSTUVWXYZ".split("")}
slug={props.slug}
/>

## UNE

### AST

Arbre de syntaxe abstraite: Représentation en arbre du code source trouvé lors d'une étape [compilation] (# compilateur) entre deux langues. Par exemple, [gatsby-transformer-remarque] (/ packages / gatsby-transformer-remarque /) créera un AST à partir de [Markdown] (# markdown) pour décrire un document de Markdown dans une arborescence en utilisant la [Remarque] (# remarque ) analyseur.

### API

Interface de programmation d'application: Méthode permettant à une application de communiquer avec une autre. Par exemple, un [plugin source] (# source-plugin) utilisera souvent une API pour obtenir ses données.

### Accessibilité

La pratique inclusive consistant à supprimer les obstacles qui empêchent les personnes handicapées d’interagir avec les sites Web ou d’y accéder. Lorsque les sites sont correctement conçus, développés et modifiés pour des raisons d'accessibilité, tous les utilisateurs ont généralement le même accès aux informations et aux fonctionnalités. Lisez à propos de [l'engagement en matière d'accessibilité de Gatsby] (/ blog / 2019-04-18-gatsby-engagement-en-accessibilité /).


## B

### Babel

Un outil qui vous permet d'écrire le plus moderne [JavaScript] (# javascript), et sur [build] (# build), il obtient [compilé] (# compilateur) pour coder de manière compréhensible pour la plupart des navigateurs Web.

### Backend

Les coulisses que le [public] (# public) ne voient pas. Cela fait souvent référence au panneau de configuration de votre [CMS] (# cms). Ceux-ci sont souvent alimentés par des langages de programmation côté serveur tels que Node.js, PHP, Go, ASP.net, Ruby ou Java.

### Construire

Dans Gatsby, il s’agit du processus consistant à intégrer votre code et votre contenu à un site Web pouvant être hébergé et consulté. Communément appelé _build time_. Voir aussi: [backend] (# backend) et [côté serveur] (# côté serveur).


## C

### Cache

Stockage local d'informations pouvant être réutilisées afin que les calculs et les recherches puissent être récupérés plus rapidement à partir d'un endroit. Gatsby utilise un cache pour stocker des informations, ce qui lui permet de créer votre site plus rapidement lorsque vous développez sans avoir à refaire le même travail deux fois.

### CLI

Interface de ligne de commande: Une application qui s'exécute sur votre ordinateur via la [ligne de commande] (# ligne de commande) et qui interagit avec votre clavier.

Gatsby a deux interfaces de ligne de commande. Un, [`gatsby`] (/ docs / gatsby-cli /), pour le développement quotidien avec Gatsby et un autre, [` gatsby-dev`] (/ contribuer / mettre en place votre local-dev- environnement / # gatsby-repo-install-instructions), pour ceux qui contribuent au projet Gatsby.

### Côté client

Côté client, on entend les opérations effectuées par le navigateur de l'utilisateur dans une [relation client-serveur] (https://en.wikipedia.org/wiki/Client%E2%80%93server_model) sur un réseau informatique. Dans Gatsby, cela est important lorsque [travailler avec les packages] (/ docs / utiliser-client-side-only-packages /) qui reposent sur des objets du [DOM navigateur] (# dom), tels que `window` ou` navigator `. Voir aussi: [côté serveur] (# côté serveur), [frontend] (# frontend) et [backend] (# fond).

### CMS

Système de gestion de contenu: application dans laquelle vous pouvez gérer votre contenu et le sauvegarder dans une base de données ou un fichier pour y accéder ultérieurement. WordPress, Drupal, Contentful et Netlify CMS sont des exemples de systèmes de gestion de contenu.

### Ligne de commande

Une interface textuelle pour exécuter des commandes sur votre ordinateur. Les applications de ligne de commande par défaut pour Mac et Windows sont respectivement `Terminal` et` Invite de commandes`.

### Compilateur

Un compilateur est un programme qui traduit le code écrit dans une langue dans une autre langue. Par exemple, [Gatsby] (# gatsby) peut compiler des applications [React] (# rea) dans des fichiers [HTML] (# html) statiques.


Composant ###

Les composants sont des fragments de code indépendants et réutilisables, générés par [React] (# react), qui, une fois combinés, constituent votre site Web ou votre application.

Un composant peut inclure des composants en son sein. En fait, [pages] (# page) et [modèles] (# modèle) sont des exemples de composants.

### Config

Le fichier de configuration, `gatsby-config.js` indique à Gatsby des informations sur votre site Web. Une option commune définie dans config est la métadonnée de votre site qui peut propulser vos balises méta SEO.

### CSS

[CSS] (https://developer.mozilla.org/en-US/docs/Web/CSS) est l'abréviation de Cascading Style Sheets. Il s'agit d'un élément essentiel de la plate-forme Web avec [HTML] (# html) et [JavaScript. ] (# javascript). CSS est un langage de style conçu pour les pages Web hautement compatibles avec les versions antérieures. À mesure que de nouvelles fonctionnalités sont proposées aux utilisateurs finaux, les [analyseurs syntaxiques CSS] (https://www.html5rocks.com/fr/tutorials/internals/howbrowserswork/#CSS_parsing) peuvent en toute sécurité ignorer les fonctionnalités non prises en charge et améliorer les propriétés qu'ils prennent en charge. . CSS y parvient avec sa conception _cascading_, essentielle pour le style avec de nouvelles techniques telles que [CSS Grid] (https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Grid_Layout/CSS_Grid_and_Progressive_Enhancement) tout en offrant des solutions de rechange aux navigateurs plus anciens. . Gatsby prend en charge plusieurs [approches de style] (/ docs / styling /), y compris des fichiers CSS normaux, des modules CSS et CSS-in-JS.



## RÉ

### La source de données

Le point d'origine du contenu et des données, généralement intégré à Gatsby avec [plugins source] (# source-plugin). Une source de données est souvent un [CMS sans tête] (# headless-cms), mais elle peut également inclure des fichiers Markdown, JSON ou YAML.

### Base de données

Une base de données est un ensemble structuré de données ou de contenu. Souvent, un [CMS] (# cms) sera sauvegardé dans une base de données utilisant [technologies de backend] (# backend). Ils sont souvent accédés dans Gatsby via un [plugin source] (# source-plugin)

### découplé

Le découplage décrit la séparation des différentes préoccupations. Avec [Gatsby] (# gatsby), cela signifie généralement le découplage de [interface] (# interface) du [backend] (# backend), comme avec [Decoupled Drupal] (https://dri.es/how-to- decouple-drupal-in-2019) ou [WordPress headless] (https://www.smashingmagazine.com/2018/10/headless-wordpress-decoupled/).

### Déployer

Le processus de [construction] (# construction) de votre site Web ou de votre application et de son téléchargement sur un [fournisseur d'hébergement] (# hébergement).

### Environnement de développement

Le [environnement] (# environnement) lorsque vous développez votre code. Il est accessible via [CLI] (# cli) en utilisant `gatsby develop` et fournit des rapports d'erreurs supplémentaires ainsi que des outils pour vous aider à déboguer avant de construire pour [production] (# production-environnement).

### DOM

Le modèle d'objet de document, généralement appelé "DOM", est une API de navigateur standard qui connecte des pages Web à des scripts ou à des langages de programmation en représentant la structure d'un document HTML en mémoire. Les développeurs interagissent généralement avec le DOM via le balisage [HTML] (# html) (écrit en [JSX] (# jsx) dans Gatsby), ainsi que les deux [React] (https://reactjs.org/docs/react-dom .html) et [JavaScript vanille] (https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model/Introduction#DOM_and_JavaScript). L’utilisation du balisage HTML [accessible] (# accessibilité) pour exposer la structure d’une page à la technologie d’assistance constitue un autre aspect important de l’exploitation optimale du DOM.


## E

### ECMAScript

ECMAScript (souvent appelé ES) est une spécification pour les langages de script. [JavaScript] (# javascript) est une implémentation de ECMAScript. Les développeurs utilisent souvent [Babel] (# babel) pour [compiler] (# compilateur) le dernier code ECMAScript en JavaScript plus largement pris en charge.

### Environnement

Environnement dans lequel Gatsby est exécuté. Par exemple, lorsque vous écrivez votre code, vous souhaitez probablement effectuer le plus de débogage possible, mais cela n’est pas souhaitable sur le site Web ou l’application en direct. En tant que tel, Gatsby peut changer de comportement en fonction de l’environnement dans lequel il évolue.

Gatsby prend en charge deux environnements par défaut, [environnement de développement] (# environnement de développement) et [environnement de production] (# environnement de production).

### Variables d'environnement

[Variables d'environnement] (/ docs / environment-variables /) vous permettent de personnaliser le comportement de votre application en fonction de son [environnement] (# environnement). Par exemple, vous souhaiterez peut-être obtenir le contenu d'un CMS intermédiaire pendant le développement et vous connecter à votre CMS de production lorsque vous [construisez] (# construisez) votre site. Avec les variables d'environnement, vous pouvez définir une URL différente pour chaque environnement.


## F

### Système de fichiers

La façon dont les fichiers sont organisés. Avec Gatsby, cela signifie que les fichiers se trouvent au même endroit que le code de votre site Web ou de votre application au lieu d'extraire des données d'une [source] externe (# source de données). L’utilisation courante du système de fichiers dans Gatsby inclut le contenu Markdown, des images, des fichiers de données et d’autres ressources.

### L'extrémité avant

L'interface [publique] de votre site Web ou de votre application, livrée à l'aide des technologies Web: HTML, CSS et JavaScript. Pour en savoir plus sur la manière dont la plate-forme Web associe ces technologies, consultez cet article sur [Fonctionnement des navigateurs] (https://www.html5rocks.com/fr/tutorials/internals/howbrowserswork/).

## G

### Gatsby

Gatsby est un framework de site Web moderne qui optimise les performances de chaque site Web ou application en exploitant les dernières technologies Web telles que [React] (# react), [GraphQL] (# graphql) et moderne [JavaScript] (# javascript). Grâce à Gatsby, il est facile de créer des expériences Web fascinantes et convaincantes sans avoir à devenir un expert en performances.

# # # GraphQL

Un langage [query] (# query) qui vous permet d'extraire des données sur votre site Web ou votre application. C’est l’interface que Gatsby utilise] (/ docs / graphql /) pour gérer les données du site.


## H

### HTML

Un langage de balisage que chaque navigateur Web est capable de comprendre. Il représente Hypertext Markup Language. [HTML] (https://developer.mozilla.org/en-US/docs/Web/HTML) donne à votre contenu Web une structure informative universelle, définissant des éléments tels que des en-têtes, des paragraphes, etc. C'est également essentiel pour fournir un site Web accessible.

### CMS sans tête

Un [CMS] (# cms) qui gère uniquement la gestion de contenu [backend] (# backend) au lieu de gérer à la fois le backend et le [frontend] (# frontend). Ce type de configuration est également appelé [Découplé] (# découplé).

### Hébergement

Un fournisseur d'hébergement conserve une copie de votre site Web ou de votre application et la rend accessible au [public] (# public). Les projets [fournisseurs d'hébergement courants pour Gatsby] (/ docs / deploying-and-hosting /) incluent Netlify, AWS, S3, Surge, Heroku, etc.

### Remplacement du module chaud

Une fonctionnalité utilisée lorsque vous exécutez `gatsby develop` et qui met à jour en direct votre site sur la sauvegarde de code dans un éditeur de texte en remplaçant automatiquement les modules, ou des morceaux de code, dans une fenêtre de navigateur ouverte.

### Hydratation

Une fois qu'un site a été [construit] (# construit) par Gatsby et chargé dans un navigateur Web, les ressources JavaScript [côté client] (# côté client) seront téléchargées et transformées en une application React complète capable de manipuler le [ DOM] (# dom). Ce processus s'appelle souvent la réhydratation, car il exécute une partie du code JavaScript utilisé pour générer les pages Gatsby, mais cette fois avec des API DOM du navigateur comme `window` disponibles.

## JE


## J

### JAMStack

JAMStack fait référence à une architecture Web moderne utilisant les balises [JavaScript] (# javascript), [APIs] (# api) et ([HTML] (# html)). [JAMStack.org] (https://jamstack.org): "Il s’agit d’un nouveau moyen de créer des sites Web et des applications offrant de meilleures performances, une sécurité accrue, un coût de dimensionnement réduit et une meilleure expérience de développeur."

### JavaScript

Un langage de programmation qui nous aide à rendre le Web dynamique et interactif. [JavaScript] (https://developer.mozilla.org/en-US/docs/Web/Javascript) est une technologie Web largement déployée dans les navigateurs. Il est également utilisé côté serveur avec [Node.js] (# node). C'est une implémentation de la spécification [ECMAScript] (# ECMAScript).

### JSX

JSX est une extension de JavaScript qui permet aux développeurs d'écrire des composants HTML et personnalisés dans le même morceau de code. L'équipe [React recommande] (https://reactjs.org/docs/introducing-jsx.html) de l'utiliser pour décrire à quoi devrait ressembler une [UI] (# UI). JSX peut vous rappeler un langage de modèle, mais il est doté de toute la puissance de JavaScript. Certains détails importants à noter sont que, du fait que JSX utilise JavaScript, certains attributs HTML de votre balisage doivent être permutés pour éviter les mots réservés dans JavaScript (des éléments tels que `htmlFor` et` className`).

## K

## L

### Linting

Linting est le processus d’exécution d’un programme qui analysera le code pour détecter les erreurs potentielles. Le projet Gatsby utilise [prettier] (https://prettier.io/) pour identifier et résoudre les problèmes de style courants. Un autre exemple de linter couramment utilisé dans les projets React est [eslint-jsx-plugin-a11y] (https://github.com/evcohen/eslint-plugin-jsx-a11y), qui vérifie les [accessibilité] courantes (accessibilité). ) problèmes en développement.


## M

### MDX

Étend [Markdown] (# markdown) pour prendre en charge [React] (# react) [composants] (# composant) dans votre contenu.

### Markdown

Une façon d'écrire du contenu HTML avec du texte brut, en utilisant des caractères spéciaux pour désigner des types de contenu tels que des symboles de hachage pour [en-têtes] (https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) et soulignés et astérisques pour mettre l’accent sur le texte.

## N

### NPM

[Noeud] (# noeud) [Package] (# package) Gestionnaire. Vous permet d'installer et de mettre à jour d'autres packages dont dépend votre projet. [Gatsby] (# gatsby) et [React] (# react) sont des exemples des dépendances de votre projet. Voir aussi: [Fil] (# fil).

### Noeud

Gatsby utilise [noeuds de données] (/ docs / node-interface /) pour représenter une seule donnée. Une [source de données] (# source de données) créera plusieurs nœuds.

### Node.js

Un programme qui vous permet d’exécuter [JavaScript] (# javascript) sur votre ordinateur. Gatsby est alimenté par Node.

## O


## P

### Paquet

Un paquet décrit généralement un programme [JavaScript] (# javascript) qui contient des informations supplémentaires sur la manière dont il doit être distribué et utilisé, tel que son numéro de version. [NPM] (# npm) et [Yarn] (# yarn) gèrent et installent les packages utilisés par votre projet. [Gatsby] (# gatsby) est lui-même un paquet.

### Page

Une page [HTML] (# html).

Cela fait aussi souvent référence aux [composants] (# composant) qui résident dans `/ src / pages /` et sont convertis en pages par [Gatsby] (# gatsby), ainsi que par des [pages créées dynamiquement] (/ docs / creation- and-modifying-pages / # creation-pages-in-gatsby-nodejs) dans votre fichier `gatsby-node.js`.

### Brancher

Code supplémentaire qui ajoute à Gatsby des fonctionnalités qui n'étaient pas incluses prêtes à l'emploi. Les [plugins Gatsby] courants (/ plugins /) incluent les plugins [source] (# source-plugins) et [transformer] (# transformer) pour extraire et manipuler des données, respectivement.

### Environnement de production

[Environnement] (# environnement) du site Web ou de l'application [construit] (# construit) que les utilisateurs pourront utiliser lorsqu'ils [déployé] (# déploient). Vous pouvez y accéder via [CLI] (# cli) en utilisant `gatsby build` ou` gatsby serve`.

### par programme

Quelque chose qui se produit automatiquement en fonction de votre code et de votre configuration. Par exemple, vous pouvez [configurer] (# config) votre projet pour créer une [page] (# page) pour chaque article de blog écrit ou lire et afficher l'année en cours avec le droit d'auteur sur le pied de page de votre site.

### Amélioration progressive

L'amélioration progressive est une stratégie pour le Web qui met l'accent sur le fait que le contenu d'une page principale est chargé à partir d'un serveur avant toute autre chose, sans qu'il soit nécessaire de charger [JavaScript] (# javascript). Cette stratégie ajoute ensuite progressivement des couches de présentation et des fonctionnalités plus complexes en plus du contenu, dans la mesure où le navigateur / la connexion réseau de l'utilisateur final le permet. L'approche par défaut de Gatsby sur les pages [construction] (# construction) en avance signifie que le contenu sera chargé en premier et amélioré en même temps que le téléchargement et l'exécution des scripts.

### Publique

Cela fait généralement référence à un membre du public (par opposition à votre équipe) ou au dossier `/ public` dans lequel votre site Web ou votre application [construit] (# construit) est enregistré.


## Q

### Requete

Le processus de demande de données spécifiques de quelque part. Avec Gatsby, vous interrogez normalement avec [GraphQL] (# graphql).

## R

### Réagir

Une bibliothèque de code (écrite avec [JavaScript] (# javascript)) pour créer des interfaces utilisateur. C’est le cadre que [Gatsby] (# gatsby) utilise pour créer des pages et structurer le contenu.

### Remarque

Un analyseur syntaxique pour traduire le code [Markdown] (# markdown) en un autre format, tel que le code [HTML] (# html) ou le code [React] (# react).

### Runtime

Le temps d'exécution est quand un programme est en cours d'exécution (ou en cours d'exécution); cela peut faire référence à quelques choses. [Node.js] (# nodejs) est un environnement d'exécution [côté serveur] (# côté serveur) qui exécute le code JavaScript. [JavaScript côté client] (# côté client), quant à lui, fait référence au moteur d'exécution où le code JavaScript traditionnel s'exécute. Gatsby compile votre site à la [date de création] (# construction) et [se réhydrate avec un runtime de React] (# hydratation) pour offrir une expérience utilisateur rapide, interactive et dynamique.

### Routing

Le routage est le mécanisme permettant de charger le contenu correct dans un site Web ou une application en fonction d'une requête réseau, généralement une URL. Par exemple, il permet de router des URL telles que `/ about-us` vers la [page] (# page), le [modèle] (# modèle) ou le [composant] (# composant) approprié.


## S

### Schema

Représentation exacte de la manière dont les données sont stockées dans un système, telles que des tables et des champs dans une base de données ou une structure de fichier JSON. Dans Gatsby, le schéma GraphQL exprime toutes les données pouvant faire l'objet d'une requête, à savoir les données que les composants peuvent interroger dans le cadre de la couche de données de Gatsby.

### Du côté serveur

La partie serveur de la [relation client-serveur] (https://en.wikipedia.org/wiki/Client%E2%80%93server_model) fait référence à des opérations effectuées par un programme informatique qui gère l'accès à une ressource centralisée ou service dans un réseau informatique. Gatsby utilise la technologie côté serveur [Node.js] (# nodejs) pour compiler les pages au moment de la construction, par opposition à leur utilisation à [navigateur] (# runtime) avec JavaScript [client] (# client) . Voir aussi: [frontend] (# frontend) et [backend] (# backend).

### Code source

Le code source est votre code qui réside dans le dossier `/ src /` et constitue les aspects uniques de votre site Web ou de votre application. Il est composé de [JavaScript] (# javascript) et parfois de [CSS] (# css) et d'autres fichiers.

Le code source obtient [build] (# build) dans le site visible par [public] (# public).

### Source Plugin

Un [plugin] (# plugin) qui ajoute des [sources de données] (# source de données) supplémentaires à Gatsby qui peut ensuite être [interrogé] (# requête) par vos [pages] (# page) et [composants] (# composant). ).

### Entrée

Un projet Gatsby préconfiguré qui peut être utilisé comme point de départ pour votre projet. Ils peuvent être découverts à l’aide de la [Bibliothèque de démarrage Gatsby] (/ starters /) et installés à l’aide de [CLI Gatsby] (/ docs / starters /).

### Statique

Gatsby [builds] (# build) versions statiques de votre page faciles à héberger (# hosting). Cela contraste avec les systèmes dynamiques dans lesquels chaque page est générée à la volée. Le fait d'être statique génère des gains de performances importants, car le travail ne doit être effectué qu'une fois par contenu ou changement de code.

Il fait également référence au dossier `/ static` qui est automatiquement copié dans` / public` sur chaque [build] (# build) pour les fichiers qui n'ont pas besoin d'être traités par Gatsby mais qui doivent exister dans [public] ( #Publique).


## T

### Modèle

Un [composant] (# composant) qui est [programmatiquement] (# programmatiquement) transformé en page par Gatsby.

### Thème

Un thème Gatsby est comme un thème WordPress qui est composable (avec d'autres thèmes), extensible (avec plus de logique) et remplaçable ([shadowing] (/ blog / 2019-04-29-composant-shadowing /)). Les thèmes Gatsby peuvent contenir n'importe quelle facette d'une application Gatsby et peuvent également offrir un nombre quelconque de boutons permettant d'activer ou de désactiver des fonctionnalités.

### Transformateur

Un [plugin] (# plugin) qui transforme un type de données en un autre. Par exemple, vous pouvez transformer une feuille de calcul en un tableau [JavaScript] (# javascript).

## U

### UI

Une interface utilisateur fait référence à une interface utilisateur. Dans le domaine de l'interaction homme-machine, une interface utilisateur est un espace où se produisent des interactions entre l'homme et les machines. Le but de cette interaction est de permettre un fonctionnement et un contrôle efficaces de la machine depuis le côté humain, tandis que la machine renvoie simultanément des informations qui facilitent le processus de prise de décision de l'utilisateur (telles que des messages d'erreur ou des notifications).


## V

## W

### Webpack

Une application [JavaScript] (# javascript) utilisée par Gatsby pour regrouper le code de votre site Web. Cela se produit automatiquement sur [build] (# build).

## X

## Y

### Fils

Un gestionnaire [package] (# package) que certains préfèrent [NPM] (# npm). Il est également requis pour [développer Gatsby] (/ contribuer / configurer-votre-environnement-dev-local / # using-yarn).

## Z
