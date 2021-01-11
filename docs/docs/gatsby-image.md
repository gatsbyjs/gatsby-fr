---
title: Gatsby Image API
---

Une partie de ce qui rend les sites Gatsby si rapides est son approche recommandée pour la gestion des images. `gatsby-image` est un composant React conçu pour fonctionner de manière transparente avec les capacités de [native image processing](https://image-processing.gatsbyjs.org/) de Gatsby optimisées par GraphQL et [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/) pour optimiser facilement et complètement le chargement d'images pour vos sites.

> _Remarque: gatsby-image n'est **pas** un remplacement instantané de `<img />`. Il est optimisé pour les images réactives de largeur / hauteur fixe et les images qui s'étendent sur toute la largeur d'un conteneur. Il existe également d'autres façons de [work with images](/docs/images-and-files/) dans Gatsby qui ne nécessitent pas GraphQL._

Demo: [https://using-gatsby-image.gatsbyjs.org/](https://using-gatsby-image.gatsbyjs.org/)

## Setting up Gatsby Image

Pour commencer à travailler avec Gatsby Image, installez le package `gatsby-image` avec les plugins nécessaires` gatsby-transformer-sharp` et `gatsby-plugin-sharp`. Faites référence aux packages dans votre fichier `gatsby-config.js`. Vous pouvez également fournir des options supplémentaires à [`gatsby-plugin-sharp`](/packages/gatsby-plugin-sharp/) dans votre fichier de configuration.

Une façon courante de générer des images est d'installer et d'utiliser `gatsby-source-filesystem` pour connecter vos fichiers locaux, mais d'autres plugins sources peuvent également être utilisés, tels que` gatsby-source-contentful`, `gatsby-source-datocms `et` gatsby-source-sanity`.

```bash
npm install --save gatsby-image gatsby-plugin-sharp gatsby-transformer-sharp
```

```js:title=gatsby-config.js
module.exports = {
  plugins: [
    `gatsby-transformer-sharp`,
    `gatsby-plugin-sharp`,
    {
      resolve: `gatsby-source-filesystem`,
      options: {
        path: `${__dirname}/src/data/`,
      },
    },
  ],
}
```

_Pour des instructions d'installation détaillées, consultez la documentation sur [Using Gatsby Image](/docs/using-gatsby-image/)._

### Gatsby image starts with a query

Pour alimenter les données du fichier dans Gatsby Image, configurez une [GraphQL query](/docs/graphql-reference/) et transmettez-la dans un composant en tant qu'accessoires ou écrivez-la directement dans le composant. Une technique consiste à tirer parti du hook [`useStaticQuery`](/docs/use-static-query/).

Les requêtes GraphQL courantes pour la recherche d'images incluent `file` de [gatsby-source-filesystem](/packages/gatsby-source-filesystem/), ainsi que` imageSharp` et `allImageSharp` de [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/), mais en fin de compte, les options disponibles dépendront de vos sources de contenu.

> Remarque: vous pouvez également utiliser les [alias GraphQL](/docs/graphql-reference/#aliasing) pour interroger plusieurs images du même type.

Voir ci-dessous pour des exemples de code de requêtes et comment les utiliser dans les composants.

## Types of images with `gatsby-image`

Les objets image Gatsby sont créés via les méthodes GraphQL. Il existe deux types d'optimisations d'image disponibles, _fixed_ et _fluid_, qui créent plusieurs tailles d'image (1x, 1,5x, etc.). Il existe également la méthode _resize_, qui renvoie une seule image.

### Images with a _fixed_ width and height

Créez automatiquement des images pour différentes résolutions à une largeur ou une hauteur définie - Gatsby crée des images réactives pour des densités de pixels 1x, 1,5x et 2x à l'aide de l'élément `<picture>`.

Une fois que vous avez demandé une image `fixed` pour récupérer ses données, vous pouvez passer ces données dans le composant `Img`:

```jsx
import { useStaticQuery, graphql } from "gatsby"
import Img from "gatsby-image"

export default () => {
  const data = useStaticQuery(graphql`
    query {
      file(relativePath: { eq: "images/default.jpg" }) {
        childImageSharp {
          # Specify a fixed image and fragment.
          # The default width is 400 pixels
          // highlight-start
          fixed {
            ...GatsbyImageSharpFixed
          }
          // highlight-end
        }
      }
    }
  `)
  return (
    <div>
      <h1>Hello gatsby-image</h1>
      <Img
        fixed={data.file.childImageSharp.fixed} {/* highlight-line */}
        alt="Gatsby Docs are awesome"
      />
    </div>
  )
}
```

#### Fixed image query parameters

Dans une requête, vous pouvez spécifier des options pour les images fixes.

- `width` (int, default: 400)
- `height` (int)
- `quality` (int, default: 50)

#### Returns

- `base64` (string)
- `aspectRatio` (float)
- `width` (float)
- `height` (float)
- `src` (string)
- `srcSet` (string)

C'est là que des fragments comme `GatsbyImageSharpFixed` sont utiles, car ils renverront tous les éléments ci-dessus sur une ligne sans avoir à tous les taper:

```graphql
file(relativePath: { eq: "images/default.jpg" }) {
  childImageSharp {
    // highlight-start
    fixed(width: 400, height: 400) {
      ...GatsbyImageSharpFixed
      // highlight-end
    }
  }
}
```

En savoir plus sur les requêtes d'images fixes dans le README [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/#fixed).

### Images that stretch across a _fluid_ container

Créez des tailles flexibles pour une image qui s'étire pour remplir son conteneur. Par exemple. pour un conteneur dont la largeur maximale est de 800px, les tailles automatiques seraient: 200px, 400px, 800px, 1200px et 1600px - suffisamment pour fournir une taille d'image proche de la taille optimale pour chaque taille de périphérique / résolution d'écran. Si vous voulez plus de contrôle sur les tailles produites, vous pouvez utiliser le paramètre `srcSetBreakpoints`.

Une fois que vous avez demandé une image `fluid` pour récupérer ses données, vous pouvez passer ces données dans le composant` Img`:

```jsx
import { useStaticQuery, graphql } from "gatsby"
import Img from "gatsby-image"

export default () => {
  const data = useStaticQuery(graphql`
    query {
      file(relativePath: { eq: "images/default.jpg" }) {
        childImageSharp {
          # Specify a fluid image and fragment
          # The default maxWidth is 800 pixels
          // highlight-start
          fluid {
            ...GatsbyImageSharpFluid
          }
          // highlight-end
        }
      }
    }
  `)
  return (
    <div>
      <h1>Hello gatsby-image</h1>
      <Img
        fluid={data.file.childImageSharp.fluid} {/* highlight-line */}
        alt="Gatsby Docs are awesome"
      />
    </div>
  )
}
```

#### Fluid image query parameters

Dans une requête, vous pouvez spécifier des options pour les images fluides.

- `maxWidth` (int, default: 800)
- `maxHeight`(int)
- `quality` (int, default: 50)
- `srcSetBreakpoints` (array of int, default: [])
- `background` (string, default: `rgba(0,0,0,1)`)

#### Returns

- `base64` (string)
- `aspectRatio` (float)
- `src` (string)
- `srcSet` (string)
- `srcSetType` (string)
- `sizes` (string)
- `originalImg` (string)

C'est là que des fragments comme `GatsbyImageSharpFluid` sont utiles, car ils renverront tous les éléments ci-dessus sur une ligne sans avoir à tous les taper:

```graphql
file(relativePath: { eq: "images/default.jpg" }) {
  childImageSharp {
    // highlight-start
    fluid(maxWidth: 400) {
      ...GatsbyImageSharpFluid
      // highlight-end
    }
  }
}
```

En savoir plus sur les requêtes d'images fluides dans le README [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/#fluid).

### Resized images

En plus des images _fixed_ et _fluid_, l'API gatsby-image vous permet d'appeler une méthode `resize` avec `gatsby-plugin-sharp` pour renvoyer une seule image par opposition à plusieurs tailles. Aucun fragment par défaut n'est disponible pour la méthode de redimensionnement.

#### Parameters

- `width` (int, default: 400)
- `height` (int)
- `quality` (int, default: 50)
- `jpegProgressive` (bool, default: true)
- `pngCompressionLevel` (int, default: 9)
- `base64`(bool, default: false)

#### Returns

Resize renvoie un objet avec les éléments suivants:

- `src` (string)
- `width` (int)
- `height` (int)
- `aspectRatio` (float)

```graphql
allImageSharp {
  edges {
    node {
        resize(width: 150, height: 150, grayscale: true) {
          src
        }
    }
  }
}
```

En savoir plus sur les requêtes d'images redimensionnées dans le README [gatsby-plugin-sharp](/packages/gatsby-plugin-sharp/#resize).

### Shared query parameters

En plus des paramètres `gatsby-plugin-sharp` dans `gatsby-config.js`, il existe des options de requête supplémentaires qui s'appliquent aux images _fluid_, _fixed_ et _resized_:

- [`grayscale`](/packages/gatsby-plugin-sharp/#grayscale) (bool, default: false)
- [`duotone`](/packages/gatsby-plugin-sharp/#duotone) (bool|obj, default: false)
- [`toFormat`](/packages/gatsby-plugin-sharp/#toformat) (string, default: \`\`)
- [`cropFocus`](/packages/gatsby-plugin-sharp/#cropfocus) (string, default: `ATTENTION`)
- [`fit`](/packages/gatsby-plugin-sharp/#fit) (string, default: `COVER`)
- [`pngCompressionSpeed`](/packages/gatsby-plugin-sharp/#pngcompressionspeed) (int, default: 4)
- [`rotate`](/packages/gatsby-plugin-sharp/#rotate) (int, default: 0)

Voici un exemple d'utilisation de l'option `duotone` avec une image fixe:

```graphql
fixed(
  width: 800,
  duotone: {
    highlight: "#f00e2e",
    shadow: "#192550"
  }
)
```

<figure>
  <img
    alt="Jay Gatsby holding wine class in normal color and duotone."
    src="./images/duotone-before-after.png"
  />
  <figcaption>Duotone | Before - After</figcaption>
</figure>

Et un exemple d'utilisation de l'option `grayscale` avec une image fixe:

```graphql
fixed(
  grayscale: true
)
```

<figure>
  <img
    alt="Jay Gatsby holding wine class in normal color and duotone."
    src="./images/grayscale-before-after.png"
  />
  <figcaption>Grayscale | Before - After</figcaption>
</figure>

En savoir plus sur les paramètres de requête d'image partagée dans le fichier README [`gatsby-plugin-sharp`](/packages/gatsby-plugin-sharp/#shared-options).

## Image query fragments

GraphQL inclut un concept appelé "fragments de requête", qui font partie d'une requête qui peut être réutilisée. Pour faciliter la construction avec `gatsby-image`, les plugins de traitement d'image Gatsby qui prennent en charge `gatsby-image` sont livrés avec des fragments que vous pouvez facilement inclure dans vos requêtes.

> Remarque: l'utilisation de fragments dans vos requêtes dépend de la ou des sources de données que vous avez configurées. En savoir plus sur les fragments de requête d'image dans le fichier README [gatsby-image](/packages/gatsby-image/#fragments).

### Common fragments with `gatsby-transformer-sharp`

#### Fixed images

- `GatsbyImageSharpFixed`
- `GatsbyImageSharpFixed_noBase64`
- `GatsbyImageSharpFixed_tracedSVG`
- `GatsbyImageSharpFixed_withWebp`
- `GatsbyImageSharpFixed_withWebp_noBase64`
- `GatsbyImageSharpFixed_withWebp_tracedSVG`

#### Fluid images

- `GatsbyImageSharpFluid`
- `GatsbyImageSharpFluid_noBase64`
- `GatsbyImageSharpFluid_tracedSVG`
- `GatsbyImageSharpFluid_withWebp`
- `GatsbyImageSharpFluid_withWebp_noBase64`
- `GatsbyImageSharpFluid_withWebp_tracedSVG`

#### About `noBase64`

Si vous ne voulez pas utiliser l '[blur-up effect](https://using-gatsby-image.gatsbyjs.org/blur-up/), choisissez le fragment avec `noBase64` à la fin.

#### About `tracedSVG`

Si vous voulez utiliser les [traced placeholder SVGs](https://using-gatsby-image.gatsbyjs.org/traced-svg/), choisissez le fragment avec `tracedSVG` à la fin.

#### About `withWebP`

Si vous souhaitez utiliser automatiquement [images WebP](https://developers.google.com/speed/webp/) lorsque le navigateur prend en charge le format de fichier, utilisez les fragments `withWebp`. Si le navigateur ne prend pas en charge WebP, `gatsby-image` reviendra au format d'image par défaut.

Voici un exemple d'utilisation d'un fragment autre que celui par défaut de `gatsby-transformer-sharp`. Assurez-vous d'en choisir une qui correspond au type d'image souhaité (_fixed_ ou _fluid_):

```graphql
file(relativePath: { eq: "images/default.jpg" }) {
  childImageSharp {
    fluid {
      // highlight-next-line
      ...GatsbyImageSharpFluid_tracedSVG
    }
  }
}
```

Pour plus d'informations sur le fonctionnement de ces options, consultez la démo de Gatsby Image: [https://using-gatsby-image.gatsbyjs.org/](https://using-gatsby-image.gatsbyjs.org/)

#### Additional plugin fragments

De plus, les plugins prenant en charge `gatsby-image` incluent actuellement [gatsby-source-contentful](/packages/gatsby-source-contentful/), [gatsby-source-datocms](https://github.com/datocms/gatsby- source-datocms) et [gatsby-source-sanity](https://github.com/sanity-io/gatsby-source-sanity). Voir le README [gatsby-image](/packages/gatsby-image/#fragments) pour plus de détails.

## Gatsby-image props

Après avoir effectué une requête, vous pouvez transmettre des options supplémentaires au composant gatsby-image.

| Name                   | Type                | Description                                                                                                                   |
| ---------------------- | ------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `fixed`                | `object`            | Data returned from the `fixed` query                                                                                          |
| `fluid`                | `object`            | Data returned from the `fluid` query                                                                                          |
| `fadeIn`               | `bool`              | Defaults to fading in the image on load                                                                                       |
| `durationFadeIn`       | `number`            | fade-in duration is set up to 500ms by default                                                                                |
| `title`                | `string`            | Passed to the rendered HTML `img` element                                                                                     |
| `alt`                  | `string`            | Passed to the rendered HTML `img` element. Defaults to an empty string, e.g. `alt=""`                                         |
| `crossOrigin`          | `string`            | Passed to the rendered HTML `img` element                                                                                     |
| `className`            | `string` / `object` | Passed to the wrapper element. Object is needed to support Glamor's css prop                                                  |
| `style`                | `object`            | Spread into the default styles of the wrapper element                                                                         |
| `imgStyle`             | `object`            | Spread into the default styles of the actual `img` element                                                                    |
| `placeholderStyle`     | `object`            | Spread into the default styles of the placeholder `img` element                                                               |
| `placeholderClassName` | `string`            | A class that is passed to the placeholder `img` element                                                                       |
| `backgroundColor`      | `string` / `bool`   | Set a colored background placeholder. If true, uses "lightgray" for the color. You can also pass in any valid color string.   |
| `onLoad`               | `func`              | A callback that is called when the full-size image has loaded.                                                                |
| `onStartLoad`          | `func`              | A callback that is called when the full-size image starts loading, it gets the parameter `{ wasCached: <boolean> }` provided. |
| `onError`              | `func`              | A callback that is called when the image fails to load.                                                                       |
| `Tag`                  | `string`            | Which HTML tag to use for wrapping elements. Defaults to `div`.                                                               |
| `objectFit`            | `string`            | Passed to the `object-fit-images` polyfill when importing from `gatsby-image/withIEPolyfill`. Defaults to `cover`.            |
| `objectPosition`       | `string`            | Passed to the `object-fit-images` polyfill when importing from `gatsby-image/withIEPolyfill`. Defaults to `50% 50%`.          |
| `loading`              | `string`            | Set the browser's native lazy loading attribute. One of `lazy`, `eager` or `auto`. Defaults to `lazy`.                        |
| `critical`             | `bool`              | Opt-out of lazy-loading behavior. Defaults to `false`. Deprecated, use `loading` instead.                                     |

Here are some usage examples:

```jsx
<Img
  fluid={data.file.childImageSharp.fluid}
  alt="Cat taking up an entire chair"
  fadeIn="false"
  className="customImg"
  placeholderStyle={{ `backgroundColor`: `black` }}
  onLoad={() => {
    // do loading stuff
  }}
  onStartLoad={({ wasCached }) => {
    // do stuff on start of loading
    // optionally with the wasCached boolean parameter
  }}
  onError={(error) => {
    // do error stuff
  }}
  Tag="custom-image"
  loading="eager"
/>
```
