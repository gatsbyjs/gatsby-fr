---
title: Configurer votre environnement de développement
typora-copy-images-to: ./
disableTableOfContents: true
---

Avant de commencer à créer votre premier site Gatsby, vous devez vous familiariser avec certaines technologies web de base et vous assurer que vous avez installé tous les outils logiciels requis.

## Familiarisez-vous avec la ligne de commande

La ligne de commande est une interface textuelle utilisée pour exécuter des commandes sur votre ordinateur. Vous le verrez également souvent sous le nom de terminal. Dans ce didacticiel, nous utiliserons les deux de manière interchangeable. C'est un peu comme utiliser le Finder sur un Mac ou l'Explorateur sur Windows. Finder et Explorer sont des exemples d'interfaces utilisateur graphiques (GUI). La ligne de commande est un moyen puissant et textuel d'interagir avec votre ordinateur.

Prenez le temps pour trouver et ouvrir l'interface de ligne de commande (CLI) de votre ordinateur. Selon le système d'exploitation que vous utilisez, voir [**instructions pour Mac**](http://www.macworld.co.uk/feature/mac-software/how-use-terminal-on-mac-3608274/), [**instructions pour Windows**](https://www.quora.com/How-do-I-open-terminal-in-windows) ou [**instructions pour Linux**](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/).

_NB: si vous êtes débutant en lignes de commande, "exécuter" une commande signifie "écrire un certain nombre d'instructions dans votre invite de commande et presser la touche Entrer". Les commandes seront indiquées dans un encadré mis en valeur, comme ceci `node --version`, mais tous les encadrés surlignés ne sont pas des commandes ! Si quelque chose est une commande, cela sera mentionné comme quelque chose que vous aurez à exécuter._

## Installer Node.js pour le système d'exploitation approprié

Node.js est un environnement qui peut exécuter du JavaScript en dehors d'un navigateur web. Gatsby est créé avec Node.js. Pour être prêt à vous lancer avec Gatsby, vous aurez besoin d'avoir une version récente de Node.js installée sur votre ordinateur. npm est inclus avec Node.js donc si vous n'avez pas npm, il y a de grandes chances pour que vous n'ayez pas Node.js non plus.

### Instructions pour Mac

Pour installer Gatsby et Node.js, il est recommandé d'utiliser [Homebrew](https://brew.sh/). Une petite configuration au début peut vous éviter quelques maux de tête plus tard !

#### Comment installer ou vérifier Homebrew sur votre ordinateur:

1. Ouvrez votre terminal.
2. Vérifiez si Homebrew est installé en exécutant la commande `brew -v`. Vous devriez voir "Homebrew" et un numéro de version.
3. Sinon, téléchargez et installez [Homebrew en suivant les instructions](https://docs.brew.sh/Installation).
4. Une fois que vous avez installé Homebrew, répétez l'étape 2 pour vérifier s'il est bien installé.

#### Installer les outils de ligne de commande Xcode:

1. Ouvrez votre terminal.
2. Installez _Xcode Command line tools_ en exécutant la commande `xcode-select --install`.
   - Si cela échoue, téléchargez-le [directement depuis le site Apple](https://developer.apple.com/download/more/), après connexion avec un compte développeur Apple
3. Après avoir été invité à démarrer l'installation, vous serez à nouveau invité à accepter une licence logicielle pour les outils à télécharger.

#### Installer Node

1. Ouvrez votre terminal
2. Exécutez `brew install node`
   - Si vous ne souhaitez pas l'installer avec Homebrew, téléchargez la dernière version de Node.js depuis [le site officiel de Node.js](https://nodejs.org/fr/), double-cliquez sur le fichier téléchargé et suivez le processus d'installation.

### Instructions pour Windows

- Téléchargez et installez la derniere version de Node.js depuis [le site officiel de Node.js](https://nodejs.org/fr/)

### Instructions pour Linux

Installez nvm (Node Version Manager) et les dépendances nécessaires. nvm est utilisé pour gérer Node.js et toutes ses versions associées.

#### Ubuntu, Debian, et autres distros basées sur `apt` :

1. Exécutez `sudo apt update` et ensuite `sudo apt -y upgrade` pour vous assurer que votre distribution Linux est parée.
2. Exécutez `sudo apt-get install curl` pour installer curl qui vous permet de transférer des données et télécharger des dépendances additionnelles.
3. Une fois l'installation terminée, exécutez `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash` pour télécharger la dernière version de nvm.
4. Pour vous assurer que cela a marché, utilisez la commande suivante : `nvm -version`. Le résultat doit être un numéro de version.
5. [Définir la version de Node.js par défaut](#set-default-nodejs-version)

#### Arch, Manjaro et autres distros basées sur `pacman` :

1. Exécutez `sudo pacman -Sy` pour vous assurer que votre distribution Linux est parée.
2. Ces distros ont directement curl d'installé, vous pouvez donc utiliser cette commande pour télécharger nvm. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`
3. Avant d'utiliser nvm, vous devez installer des dépendances additionnelles en exécutant `sudo pacman -S grep awk tar`.
4. Pour vous assurer que cela a marché, utilisez la commande suivante : `nvm -version`. Le résultat doit être un numéro de version.
5. [Définir la version de Node.js par défaut](#set-default-nodejs-version)

#### Fedora, RedHat, and other `dnf` based distros:

1. Ces distros ont directement curl d'installé, vous pouvez donc utiliser cette commande pour télécharger nvm. `curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash`
2. Pour vous assurer que cela a marché, utilisez la commande suivante : `nvm -version`. Le résultat doit être un numéro de version.
3. [Définir la version de Node.js par défaut](#set-default-nodejs-version)

Si votre distribution Linux n'est pas listée ici, vous devriez trouver des instructions sur le web.

#### Définir la version par défaut de Node.js

Lorsque nvm est installé, il n'est pas par défaut attaché à une version particulière de node. Vous devrez installer la version que vous souhaitez utiliser et donner des instructions à nvm pour l'utiliser. Cet exemple utiliser la dernière version de Node 10, mais des versions plus récentes peuvent être utilisées à la place.

```shell
nvm install 10
nvm use 10
```

Afin de confirmer que cela a marché vous pouvez exécuter `npm --version` et `node --version`. Le résultat devrait ressembler à quelque chose comme la capture d'écran ci-dessous, montrant le numéro de version en réponse à ces commandes.

![Vérifier les versions de node et npm dans le terminal](01-node-npm-versions.png)

Une fois que vous avez suivi ces étapes d'installation et que vous avez vérifié que tout était installé correctement, vous pouvez suivre l'étape suivante.

## Installer Git

Git est un système de contrôle de version distribué gratuit et open source conçu pour gérer tout, des petits aux très grands projets avec rapidité et efficacité. Lorsque vous installez un site "starter" Gatsby, Gatsby utilise Git en arrière-plan pour télécharger et installer les fichiers requis pour votre starter. Vous devrez avoir installé Git pour configurer votre premier site Gatsby.

Les étapes pour télécharger et installer Git dépendent de votre système d'exploitation. Suivez le guide de votre système:

- [Installer Git sur macOS](https://www.atlassian.com/git/tutorials/install-git#mac-os-x)
- [Installer Git sur Windows](https://www.atlassian.com/git/tutorials/install-git#windows)
- [Installer Git sur Linux](https://www.atlassian.com/git/tutorials/install-git#linux)

## Utilisation de la CLI Gatsby

L'outil CLI de Gatsby vous permet de créer rapidement de nouveaux sites propulsés par Gatsby et d'exécuter des commandes pour développer des sites Gatsby. Il s'agit d'un package npm publié.

La CLI Gatsby est disponible via npm et doit être installée globalement en exécutant `npm install -g gatsby-cli`.

_**Remarque**: lorsque vous installez Gatsby et l'exécutez pour la première fois, vous verrez un court message vous informant des données d'utilisation anonymes qui sont collectées pour les commandes Gatsby, vous pouvez en savoir plus sur la façon dont ces données sont extraites et utilisées dans le [doc de télémétrie](/docs/telemetry)._

Pour voir les commandes disponibles, exécutez `gatsby --help`.

![Vérifier les commandes Gatsby dans le terminal](05-gatsby-help.png)

> 💡 Si vous ne parvenez pas à exécuter correctement l'interface de ligne de commande Gatsby en raison d'un problème d'autorisations, vous souhaiterez peut-être consulter la [documentation npm sur la fixation des autorisations](https://docs.npmjs.com/getting-started/fixing-npm-permissions), ou [ce guide](https://github.com/sindresorhus/guides/blob/master/npm-global-without-sudo.md).

## Créer un site Gatsby

Vous êtes maintenant prêt à utiliser l'outil CLI Gatsby pour créer votre premier site Gatsby. À l'aide de l'outil, vous pouvez télécharger des "starters" (sites partiellement construits avec une configuration par défaut) pour vous aider à accélérer la création d'un certain type de site. Le démarreur "Hello World" que vous utiliserez ici est un démarreur avec l'essentiel nécessaire pour un site Gatsby.

1.  Ouvrez votre terminal.
2.  Exécutez `gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world`. (_Remarque: En fonction de votre vitesse de téléchargement, le temps que cela prendra variera. Par souci de concision, le gif ci-dessous a été suspendu pendant une partie de l'installation_).
3.  Exécutez `cd hello-world`.
4.  Exécutez `gatsby develop`.

<video controls="controls" autoplay="true" loop="true">
  <source type="video/mp4" src="./03-create-site.mp4" />
  <p>Désolé! Votre navigateur ne prend pas en charge cette vidéo.</p>
</video>

Qu'est-ce qui vient de se passer?

```shell
gatsby new hello-world https://github.com/gatsbyjs/gatsby-starter-hello-world
```

- `new` est une commande gatsby pour créer un nouveau projet Gatsby.
- Ici, `hello-world` est un titre arbitraire - vous pouvez choisir n'importe quoi. L'outil CLI placera le code de votre nouveau site dans un nouveau dossier appelé “hello-world”.
- Enfin, l'URL GitHub spécifiée pointe vers un référentiel de code qui contient le code de démarrage que vous souhaitez utiliser.

```shell
cd hello-world
```

- Cela dit : 'Je veux changer les répertoires («cd») en sous-dossier «hello-world». Chaque fois que vous souhaitez exécuter des commandes pour votre site, vous devez être dans le contexte de ce site (alias votre terminal doit être pointé vers le répertoire où réside le code de votre site).

```shell
gatsby develop
```

- Cette commande démarre un serveur de développement. Vous pourrez voir et interagir avec votre nouveau site dans un environnement de développement local (sur votre ordinateur, non publié sur Internet).

### Affichez votre site localement

Ouvrez un nouvel onglet dans votre navigateur et accédez à [**http://localhost:8000**](http://localhost:8000/).

![Vérifier la page d'accueil](04-home-page.png)

Félicitations! C'est le début de votre tout premier site Gatsby!🎉

Vous pourrez visiter le site localement à [**_http://localhost:8000_**](http://localhost:8000/) tant que votre serveur de développement est en cours d'exécution. C’est le processus que vous avez commencé en exécutant la commande `gatsby develop`. Pour arrêter l'exécution de ce processus (ou pour «arrêter l'exécution du serveur de développement»), revenez à la fenêtre de votre terminal, maintenez enfoncée la touche "control" puis appuyez sur «c» (ctrl-c). Pour le redémarrer, lancez à nouveau la commande `gatsby develop`!

**Remarque:** Si vous utilisez une configuration VM comme`vagrant` et / ou souhaitez écouter sur votre adresse IP locale, exécutez `gatsby develop -- --host=0.0.0.0`. Maintenant, le serveur de développement écoute à la fois sur «localhost» et sur votre IP locale.

## Configurer un éditeur de code

Un éditeur de code est un programme spécialement conçu pour éditer du code informatique. Il y en a beaucoup là-bas.

### Télécharger VS Code

La documentation de Gatsby inclut parfois des captures d'écran qui ont été prises dans VS Code, donc si vous n'avez pas encore d'éditeur de code préféré, l'utilisation de VS Code s'assurera que votre écran ressemble exactement aux captures d'écran du didacticiel et des documents. Si vous choisissez d'utiliser VS Code, visitez le site [VS Code](https://code.visualstudio.com/#alt-downloads) et téléchargez la version appropriée pour votre plate-forme.

### Installez le plugin Prettier

Nous vous recommandons également d'utiliser [Prettier](https://github.com/prettier/prettier), un outil qui permet de formater votre code pour éviter les erreurs.

Vous pouvez utiliser Prettier directement dans votre éditeur à l'aide du [plugin Prettier VS Code](https://github.com/prettier/prettier-vscode):

1.  Ouvrez la vue des extensions sur VSCode (View => Extensions).
2.  Recherchez "Prettier - Code formatter".
3.  Cliquez sur "Install". (Après l'installation, vous serez invité à redémarrer VS Code pour activer l'extension. Les versions plus récentes de VS Code activeront automatiquement l'extension après le téléchargement.)

> 💡 Si vous n'utilisez pas VS Code, consultez la documentation Prettier pour [les instructions d'installation](https://prettier.io/docs/en/install.html) ou [autres intégrations d'éditeur](https://prettier.io/docs/en/editors.html).

## ➡️ Et après?

Pour résumer, dans cette section vous:

- Connaître la ligne de commande et comment l'utiliser
- Installation et apprentissage de Node.js et de l'outil CLI npm, du système de contrôle de version Git et de l'outil CLI Gatsby
- Généré un nouveau site Gatsby à l'aide de l'outil CLI Gatsby
- Exécutez le serveur de développement Gatsby et visitez votre site localement
- Téléchargez un éditeur de code
- Installation d'un formateur de code appelé Prettier

Maintenant, passez à [**apprendre à connaître les blocs de construction de Gatsby**](/tutorial/part-one/).

## Références

### Aperçu des technologies de base

Il n’est pas nécessaire d’être déjà expert en la matière - sinon, ne vous inquiétez pas! Vous en apprendrez beaucoup au cours de cette série de didacticiels. Voici quelques-unes des principales technologies Web que vous utiliserez lors de la création d'un site Gatsby:

- **HTML**: Un langage de balisage que chaque navigateur Web est capable de comprendre. Il signifie HyperText Markup Language. HTML donne à votre contenu Web une structure d'information universelle, définissant des éléments tels que des titres, des paragraphes, etc.
- **CSS**: Un langage de présentation utilisé pour styliser l'apparence de votre contenu Web (polices, couleurs, mise en page, etc.). Il signifie feuilles de style en cascade.
- **JavaScript**: Un langage de programmation qui nous aide à rendre le Web dynamique et interactif.
- **React**: Une librairie (construite avec JavaScript) pour créer des interfaces utilisateur. C'est le cadre que Gatsby utilise pour créer des pages et structurer le contenu.
- **GraphQL**: Un langage de requête qui vous permet d'extraire des données dans votre site Web. Il s'agit de l'interface que Gatsby utilise pour gérer les données du site.

### Qu'est-ce qu'un site Web?

Pour une introduction complète à ce qu'est un site Web - y compris une introduction à HTML et CSS - consultez “[**Création de votre première page Web**](https://learn.shayhowe.com/html-css/building-your-first-web-page/)”. C'est un excellent endroit pour commencer à découvrir le Web. Pour une introduction plus pratique à [**HTML**](https://www.codecademy.com/learn/learn-html), [**CSS**](https://www.codecademy.com/learn/learn-css), et [**JavaScript**](https://www.codecademy.com/learn/introduction-to-javascript), consultez les tutoriels de Codecademy. [**React**](https://reactjs.org/tutorial/tutorial.html) et [**GraphQL**](http://graphql.org/graphql-js/) ont également leurs propres tutoriels d'introduction.

### En savoir plus sur la ligne de commande

Pour une excellente introduction à l'utilisation de la ligne de commande, consultez [**Tutoriel Command Line de Codecademy**](https://www.codecademy.com/courses/learn-the-command-line/lessons/navigation/exercises/your-first-command) pour les utilisateurs Mac et Linux, et [**ce tutorial**](https://www.computerhope.com/issues/chusedos.htm) pour les utilisateurs de Windows. Même si vous êtes un utilisateur Windows, la première page du didacticiel Codecademy est une lecture précieuse. Il explique ce qu'est la ligne de commande, pas seulement comment s'interfacer avec elle.

### En savoir plus sur npm

npm est un gestionnaire de packages JavaScript. Un package est un module de code que vous pouvez choisir d'inclure dans vos projets. Si vous venez de télécharger et d'installer Node.js, npm a été installé avec!

npm a trois composants distincts: le site Web npm, le registre npm et l'interface de ligne de commande (CLI) npm.

- Sur le site Web de npm, vous pouvez parcourir les packages JavaScript disponibles dans le registre npm.
- Le registre npm est une grande base de données d'informations sur les packages JavaScript disponibles sur npm.
- Une fois que vous avez identifié le package souhaité, vous pouvez utiliser la CLI npm pour l'installer dans votre projet ou globalement (comme les autres outils CLI). La CLI npm est ce qui parle au registre.
- vous n'interagissez généralement qu'avec le site Web npm ou la CLI npm.

> 💡 Découvrez l'introduction de npm, “[**Qu'est-ce que npm?**](https://docs.npmjs.com/getting-started/what-is-npm)”.

### En savoir plus sur Git

Vous n'aurez pas besoin de connaître Git pour terminer ce tutoriel, mais c'est un outil très utile. Si vous souhaitez en savoir plus sur le contrôle de version, Git et GitHub, consultez GitHub [Manuel Git](https://guides.github.com/introduction/git-handbook/).
