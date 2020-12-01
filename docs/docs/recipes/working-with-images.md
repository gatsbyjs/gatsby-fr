---
title: "Recettes: travailler avec des images"
tableOfContentsDepth: 1
---

Accédez aux images en tant que ressources statiques ou automatisez le processus de leur optimisation grâce à de puissants plugins.

## Importer une image dans un composant avec webpack

Les images peuvent être importées directement dans un module JavaScript avec webpack. Ce processus réduit et copie automatiquement l'image dans le dossier `public` de votre site, en fournissant une URL d'image dynamique que vous pouvez passer à un élément HTML` <img> `comme un chemin de fichier normal.

<EggheadEmbed
  leçonLink = "https://egghead.io/lessons/gatsby-import-a-local-image-into-a-gatsby-component-with-webpack"
  LessonTitle = "Importer une image locale dans un composant Gatsby avec webpack"
/>

### Conditions préalables

- Un [Site Gatsby] (/ docs / quick-start) avec un fichier `.js` exportant un composant React
- une image (`.jpg`,` .png`, `.gif`,` .svg`, etc.) dans le dossier `src`

### Les directions

1. Importez votre fichier depuis son chemin dans le dossier `src`:

``` jsx: titre = src / pages / index.js
importer React de "react"
// Dites à Webpack que ce fichier JS utilise cette image
importer FiestaImg depuis "../assets/fiesta.jpg" // surligner
```

2. Dans `index.js`, ajoutez une balise` <img> `avec le` src` comme nom de l'importation que vous avez utilisé à partir de webpack (dans ce cas `FiestaImg`), et ajoutez un attribut` alt` [décrivant l'image] (https://webaim.org/techniques/alttext/):

``` jsx: titre = src / pages / index.js
importer React de "react"
importer FiestaImg depuis "../assets/fiesta.jpg"

Exporter la fonction par défaut Accueil () {
  revenir (
    // Le résultat de l'importation est l'URL de votre image
    <img src = {FiestaImg} alt = "Un chien souriant dans un chapeau de fête" /> // surlignage
  )
}
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement.
4. Affichez votre image dans le navigateur: `http: // localhost: 8000 /`

- [Exemple de référentiel d'importation d'une image avec webpack] (https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-webpack-image)
- [En savoir plus sur toutes les techniques d'image dans Gatsby] (/ docs / images-and-files /)

## Référencez une image du dossier `static`

Comme alternative à l'importation d'actifs avec webpack, le dossier `static` permet d'accéder au contenu qui est automatiquement copié dans le dossier` public` une fois construit.

Il s'agit d'une **route d'échappement** pour [cas d'utilisation spécifiques] (/ docs / dossier-statique / # quand-utiliser-le-dossier-statique), et d'autres méthodes comme [importation avec webpack] (# import- an-image-into-a-component-with-webpack) sont recommandés pour tirer parti des optimisations faites par Gatsby.

<EggheadEmbed
  leçonLink = "https://egghead.io/lessons/gatsby-use-a-local-image-from-the-static-folder-in-a-gatsby-component"
  LessonTitle = "Utiliser une image locale du dossier statique dans un composant Gatsby"
/>

### Conditions préalables

- Un [Site Gatsby] (/ docs / quick-start) avec un fichier `.js` exportant un composant React
- Une image (`.jpg`,` .png`, `.gif`,` .svg`, etc.) dans le dossier `static`

### Les directions

1. Assurez-vous que l'image se trouve dans votre dossier `static` à la racine du projet. La structure de votre projet peut ressembler à ceci:

```texte
├── gatsby-config.js
├── src
│ └── pages
│ └── index.js
├── statique
│ └── fiesta.jpg
```

2. Dans `index.js`, ajoutez une balise` <img> `avec` src` comme chemin relatif du fichier depuis le dossier `static`, et ajoutez un attribut` alt` [décrivant l'image] (https : //webaim.org/techniques/alttext/):

``` jsx: titre = src / pages / index.js
importer React de "react"

Exporter la fonction par défaut Accueil () {
  revenir (
    <img src = {`fiesta.jpg`} alt =" Un chien souriant dans un chapeau de fête "/> // surlignage
  )
}
```

3. Exécutez `gatsby develop` pour démarrer le serveur de développement.
4. Affichez votre image dans le navigateur: `http: // localhost: 8000 /`

### Ressources additionnelles

- [Exemple de référentiel référençant une image du dossier statique] (https://github.com/gatsbyjs/gatsby/tree/master/examples/recipe-static-image)
- [Utilisation du dossier statique] (/ docs / static-folder /)
- [En savoir plus sur toutes les techniques d'image dans Gatsby] (/ docs / images-and-files /)

## Optimiser et interroger des images locales avec gatsby-image

Le plugin `gatsby-image` peut soulager une grande partie de la douleur associée à l'optimisation des images de votre site.

Gatsby générera des ressources optimisées qui pourront être interrogées avec GraphQL et transmises au composant image de Gatsby. Cela prend en charge les tâches lourdes, notamment la création de plusieurs tailles d'image et leur chargement au bon moment.

### Conditions préalables

- Les packages `gatsby-image`,` gatsby-transformer-sharp` et `gatsby-plugin-sharp` installés et ajoutés au tableau des plugins dans` gatsby-config`
- [Images provenant] (/ packages / gatsby-image / # install) dans votre `gatsby-config` en utilisant un plugin comme` gatsby-source-filesystem`

### Les directions

1. Tout d'abord, importez «Img» depuis «gatsby-image», ainsi que «graphql» et «useStaticQuery» depuis «gatsby»

```jsx
importer {useStaticQuery, graphql} depuis "gatsby" // pour rechercher des données d'image
importer des images depuis "gatsby-image" // pour prendre des données d'image et les rendre
```

2. Écrivez une requête pour obtenir des données d'image et passez les données dans le composant `<Img />`:

Choisissez l'une des options suivantes ou une combinaison d'entre elles.

une. une seule image interrogée par son fichier [chemin] (/ docs / content-and-data /) (Exemple: `images / corgi.jpg`)

```jsx
const data = useStaticQuery (graphql`
  requete {
    file (relativePath: {eq: "corgi.jpg"}) {// surligné
      childImageSharp {
        fluide {
          base64
          ratio d'aspect
          src
          srcSet
          les tailles
        }
      }
    }
  }
`)

revenir (
  <Img fluid = {data.file.childImageSharp.fluid} alt = "Un corgi souriant joyeusement" />
)
```

b. en utilisant un [fragment GraphQL] (/ docs / using-fragments /), pour interroger les champs nécessaires de manière plus concise

```jsx
const data = useStaticQuery (graphql`
  requete {
    file (relativePath: {eq: "corgi.jpg"}) {
      childImageSharp {
        fluide {
          ... GatsbyImageSharpFluid // ligne de surbrillance
        }
      }
    }
  }
`)

revenir (
  <Img fluid = {data.file.childImageSharp.fluid} alt = "Un corgi souriant joyeusement" />
)
```

c. plusieurs images d'un répertoire (Exemple: `images / dogs`) [filtré] (/ docs / graphql-reference / # filter) par les champs` extension` et `relativeDirectory`, puis mappés dans les composants` Img`

```jsx
const data = useStaticQuery (graphql`
  requete {
    allFile (
      // démarrage en surbrillance
      filtre: {
        extension: {regex: "/ (jpg) | (png) | (jpeg) /"}
        relativeDirectory: {eq: "dogs"}
      }
      // surlignage
    ) {
      bords {
        nœud {
          base
          childImageSharp {
            fluide {
              ... GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`)

revenir (
  <div>
    // démarrage en surbrillance
    {data.allFile.edges.map (image => (
      <Img
        fluide = {image.node.childImageSharp.fluid}
        alt = {image.node.base.split (".") [0]} // n'utilise que la section de l'extension de fichier avec le nom de fichier
      />
    ))}
    // surlignage
  </div>
)
```

**Remarque**: Cette méthode peut rendre difficile la correspondance des images avec le texte `alt` pour l'accessibilité. Cet exemple utilise des images avec le texte `alt` inclus dans le nom de fichier, comme` dog in a party hat.jpg`.

ré. une image de taille fixe utilisant le champ «fixed» au lieu de «fluid»

```jsx
const data = useStaticQuery (graphql`
  requete {
    file (relativePath: {eq: "corgi.jpg"}) {
      childImageSharp {
        fixe (largeur: 250, hauteur: 250) {// ligne de surbrillance
          ... GatsbyImageSharpFixed
        }
      }
    }
  }
`)
revenir (
  <Img fixed = {data.file.childImageSharp.fixed} alt = "Un corgi souriant joyeusement" />
)
```

e. une image de taille fixe avec un `maxWidth`

```jsx
const data = useStaticQuery (graphql`
  requete {
    file (relativePath: {eq: "corgi.jpg"}) {
      childImageSharp {
        fixed (maxWidth: 250) {// ligne de surbrillance
          ... GatsbyImageSharpFixed
        }
      }
    }
  }
`)
revenir (
  <Img fixed = {data.file.childImageSharp.fixed} alt = "Un corgi souriant joyeusement" /> // surlignage
)
```

F. une image remplissant un récipient de fluide avec une largeur max (en pixels) et une qualité supérieure (la valeur par défaut est 50 soit 50%)

```jsx
const data = useStaticQuery (graphql`
  requete {
    file (relativePath: {eq: "corgi.jpg"}) {
      childImageSharp {
        fluid (maxWidth: 800, qualité: 75) {// ligne de surbrillance
          ... GatsbyImageSharpFluid
        }
      }
    }
  }
`)

revenir (
  <Img fluid = {data.file.childImageSharp.fluid} alt = "Un corgi souriant joyeusement" />
)
```

3. (Facultatif) Ajoutez des styles en ligne à `<Img />` comme vous le feriez avec d'autres composants

```jsx
<Img
  fluide = {data.file.childImageSharp.fluid}
  alt = "Un corgi souriant joyeusement"
  style = {{border: "2px solid rebeccapurple", borderRadius: 5, height: 250}} // ligne de surbrillance
/>
```

4. (Facultatif) Forcer une image dans un rapport hauteur / largeur souhaité en remplaçant le champ `aspectRatio` renvoyé par la requête GraphQL avant qu'il ne soit passé dans le composant` <Img /> `

```jsx
<Img
  fluide = {{
    ... data.file.childImageSharp.fluid,
    aspectRatio: 1,6, // 1280/800 = 1,6
  }}
  alt = "Un corgi souriant joyeusement"
/>
```

5. Exécutez `gatsby develop`, pour générer des images à partir de fichiers dans le système de fichiers (si ce n'est déjà fait) et mettez-les en cache

### Ressources additionnelles

- [Exemple de référentiel illustrant ces exemples] (https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [API Gatsby Image] (/ docs / gatsby-image /)
- [Utilisation de l'image Gatsby] (/ docs / using-gatsby-image)
- [En savoir plus sur l'utilisation d'images dans Gatsby] (/ docs / working-with-images /)
- [Vidéos gratuites de egghead.io expliquant ces étapes] (https://egghead.io/playlists/using-gatsby-image-with-gatsby-ea85129e)

## Optimiser et interroger les images dans le post frontmatter avec gatsby-image

Pour des cas d'utilisation comme une image en vedette dans un article de blog, vous pouvez _toujours_ utiliser `gatsby-image`. Le composant `Img` a besoin de données d'image traitées, qui peuvent provenir d'un fichier local (ou distant), y compris d'une URL dans le frontmatter d'un fichier` .md` ou `.mdx`.

Pour insérer des images dans markdown (en utilisant la syntaxe `! [] ()`), Pensez à utiliser un plugin comme [`gatsby-remarque-images`] (/ packages / gatsby-remarque-images /)

### Conditions préalables

- Les packages `gatsby-image`,` gatsby-transformer-sharp` et `gatsby-plugin-sharp` installés et ajoutés au tableau des plugins dans` gatsby-config`
- [Images provenant] (/ packages / gatsby-image / # install) dans votre `gatsby-config` en utilisant un plugin comme` gatsby-source-filesystem`
- Fichiers Markdown provenant de votre `gatsby-config` avec des URL d'image en frontmatter
- [Pages créées] (/ docs / creation-and-modifying-pages /) depuis Markdown en utilisant [`createPages`] (https://www.gatsbyjs.com/docs/node-apis/#createPages)

### Les directions

1. Vérifiez que le fichier Markdown a une URL d'image avec un chemin valide vers un fichier image dans votre projet

``` mdx: titre = post.mdx
---
title: Mon premier message
featureImage: ./corgi.png // ligne de surbrillance
---

Publier un contenu...
```

2. Vérifiez qu'un identifiant unique (un slug dans cet exemple) est passé en contexte lorsque `createPages` est appelé dans` gatsby-node.js`, qui sera ensuite passé dans une requête GraphQL dans le composant Layout

``` js: titre = gatsby-node.js
exports.createPages = async ({graphql, actions}) => {
  const {createPage} = actions

  // requête pour tous les démarques

  result.data.allMdx.edges.forEach (({node}) => {
    créer une page({
      chemin: node.fields.slug,
      composant: path.resolve (`. / src / components / markdown-layout.js`),
      // démarrage en surbrillance
      le contexte: {
        slug: node.fields.slug,
      },
      // surlignage
    })
  })
}
```

3. Maintenant, importez `Img` de` gatsby-image` et `graphql` de` gatsby` dans le composant de modèle, écrivez une [pageQuery] (/ docs / page-query /) pour obtenir des données d'image basées sur les dans `slug` et transmettez ces données au composant` <Img /> `:

``` jsx: title = markdown-layout.jsx
importer React de "react"
importer {graphql} depuis "gatsby" // ligne de surbrillance
importer des images depuis "gatsby-image" // surligner

Exporter la mise en page de la fonction par défaut ({enfants, données}) {
  revenir (
    <main>
      // démarrage en surbrillance
      <Img
        fluid = {data.markdown.frontmatter.image.childImageSharp.fluid}
        alt = "Un corgi souriant joyeusement"
      />
      // surlignage
      {enfants}
    </main>
  )
}

// démarrage en surbrillance
export const pageQuery = graphql`
  query PostQuery ($ slug: String) {
    markdown: mdx (champs: {slug: {eq: $ slug}}) {
      id
      frontmatter {
        image {
          childImageSharp {
            fluide {
              ... GatsbyImageSharpFluid
            }
          }
        }
      }
    }
  }
`
// surlignage
```
4. Exécutez `gatsby develop`, qui générera des images pour les fichiers provenant du système de fichiers

### Ressources additionnelles

- [Exemple de référentiel utilisant cette recette] (https://github.com/gatsbyjs/gatsby/tree/master/examples/recipes-gatsby-image)
- [Images en vedette avec frontmatter] (/ docs / working-with-images-in-markdown / # features-images-with-frontmatter-metadata)
- [API Gatsby Image] (/ docs / gatsby-image /)
- [Utilisation de l'image Gatsby] (/ docs / using-gatsby-image)
- [En savoir plus sur l'utilisation d'images dans Gatsby] (/ docs / working-with-images /)
