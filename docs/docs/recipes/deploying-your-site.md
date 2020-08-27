---
title: "Recettes: déployer votre site"
tableOfContentsDepth: 1
---

Showtime. Une fois que vous êtes satisfait de votre site, vous êtes prêt à l'utiliser!

## Préparation du déploiement

### Conditions préalables

- Un [site Gatsby](/docs/quick-start)
- La [CLI Gatsby](/docs/gatsby-cli) installé

### Directions

1. Arrêtez votre serveur de développement s'il est en cours d'exécution (`Ctrl + C` sur votre terminal dans la plupart des cas)

2. Pour un chemin de site standard dans le répertoire racine (`/`), lancez `gatsby build` à l'aide de l'interface de ligne de commande Gatsby sur la ligne de commande. Les fichiers créés seront désormais dans le dossier `public`.

```shell
gatsby build
```

3. Pour inclure un chemin d'accès au site autre que `/` (comme `/site-name/`), définissez un préfixe de chemin en ajoutant ce qui suit à votre `gatsby-config.js` et remplacer `yourpathprefix` par le préfixe de chemin souhaité:

```js:title=gatsby-config.js
module.exports = {
  pathPrefix: `/yourpathprefix`,
}
```

Il y a plusieurs raisons de le faire -- par exemple, héberger un blog construit avec Gatsby sur un domaine avec un autre site non construit sur Gatsby. Le site principal dirigerait vers `example.com`, et le site Gatsby avec un préfixe de chemin pourrait être à `example.com/blog`.

4. Avec un préfixe de chemin défini dans `gatsby-config.js`, lancez `gatsby build` avec le flag `--prefix-paths` pour ajouter automatiquement le préfixe au début de toutes les URLs des sites Gatsby et balises `<Link>`.

```shell
gatsby build --prefix-paths
```

5. Assurez-vous que votre site a la même apparence lors de l'exécution `gatsby build` comme avec `gatsby develop`. En exécutant `gatsby serve` lorsque vous créez votre site, vous pouvez tester (et déboguer si nécessaire) le produit fini avant de le déployer en direct.

```shell
gatsby build && gatsby serve
```

### Ressources supplémentaires

- Présentation de la création et du déploiement d'un exemple de site dans [première partie du didacticiel](/tutorial/part-one/#deploying-a-gatsby-site)
- En savoir plus sur l'[optimisation des performances](/docs/performance/)
- En savoir plus sur d'[autres sujets liés au déploiement](/docs/preparing-for-deployment/)
- Consultez la [documentation de déploiement](/docs/deploying-and-hosting/) pour des plates-formes d'hébergement spécifiques et comment y déployer 

## Déploiement sur Netlify

Utilisation [`netlify-cli`](https://www.netlify.com/docs/cli/) pour déployer votre application Gatsby sans quitter l'interface de ligne de commande.

### Conditions préalables

- Un [site Gatsby](/docs/quick-start) avec un seul composant `index.js`
- Le paquet [netlify-cli](https://www.npmjs.com/package/netlify-cli) installé
- La [CLI Gatsby](/docs/gatsby-cli) installé

### Instructions

1. Créez votre application Gatsby en utilisant `gatsby build`

2. Connectez-vous à Netlify en utilisant `netlify login`

3. Exécutez la commande `netlify init`. Sélectionnez l'option "Créer et configurer un nouveau site".

4. Choisissez un nom de site Web personnalisé si vous le souhaitez ou appuyez sur Entrée pour en recevoir un au hasard.

5. Choisissez votre [Équipe](https://www.netlify.com/docs/teams/).

6. Modifiez le chemin de déploiement en `public/`

7. Assurez-vous que tout va bien avant de déployer en production en utilisant `netlify deploy -d . --prod`

### Ressources supplémentaires

- [Hébergement sur Netlify](/docs/hosting-on-netlify)
- [gatsby-plugin-netlify](/packages/gatsby-plugin-netlify)

## Déployer sur ZEIT Now

Utilisez [Now CLI](https://zeit.co/download) pour déployer votre application Gatsby sans quitter l'interface de ligne de commande.

### Conditions préalables

- Un compte [ZEIT Now](https://zeit.co/signup)
- Un [site Gatsby](/docs/quick-start) avec un seul composant `index.js`
- [Now CLI](https://zeit.co/download) installé
- [Gatsby CLI](/docs/gatsby-cli) installé

### Instructions

1. Connectez-vous à Now CLI en utilisant `now login`

2. Accédez au répertoire de votre application Gatsby.js dans le Terminal si vous n'y êtes pas déjà

3. Lancez `now` pour le déployer

### Ressources supplémentaires

- [Déploiement sur ZEIT Now](/docs/deploying-to-zeit-now/)

## Déploiement sur Cloudflare Workers

Utilisez [`wrangler`](https://developers.cloudflare.com/workers/tooling/wrangler/) pour déployer votre application Gatsby à l'échelle mondiale sans quitter l'interface de ligne de commande.

### Conditions préalables

- Un compte sur [Cloudflare](https://dash.cloudflare.com/sign-up)
- Un [Plan Travailleurs Illimité](https://developers.cloudflare.com/workers/about/pricing/) pour \$5/mois afin d'activer le magasin KV, qui est nécessaire pour servir les fichiers Gatsby.
- Un [site Gatsby](/docs/quick-start) configuré avec la CLI de Gatsby
- [wrangler](https://developers.cloudflare.com/workers/tooling/wrangler/install/) installé globalement (`npm i -g @cloudflare/wrangler`)

### Instructions

1. Créez votre application Gatsby en utilisant `gatsby build`
2. Lancez `wrangler config` où vous serez invité à entrer votre [jeton d'API Cloudflare](https://developers.cloudflare.com/workers/quickstart/#api-token)
3. Lancez `wrangler init --site`
4. Configurez `wrangler.toml`. Ajoutez d'abord un [numéro de compte](https://developers.cloudflare.com/workers/quickstart/#account-id-and-zone-id) champ puis soit
   1. Un domaine workers.dev gratuit en définissant `workers_dev = true`
   2. Un domaine personnalisé sur Cloudflare en définissant `workers_dev = false`, `zone_id = "abdc..` et `route = customdomain.com/*`
5. Dans `wrangler.toml` définissez `bucket = "./public"`
6. Lancez `wrangler publish` et votre site sera déployé en quelques secondes!

### Ressources supplémentaires

- [Hébergement sur Cloudflare](/docs/deploying-to-cloudflare-workers)
