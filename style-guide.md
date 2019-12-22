# Style Guide

Use this file for language-specific style rules to follow for translation.

## Rules

### Text in Code Blocks

Leave text in code blocks untranslated except for comments. You may optionally translate text in strings, but be careful not to translate strings that refer to code!

Exemple:

```js
// Exemple
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Hello Gatsby!</div>
)
```

✅ DO:

```js
// Exemple
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Salut Gatsby!</div>
)
```

✅ OK:

```js
// Exemple
import React from "react"
export default () => (
  <div style={{ color: `purple`, fontSize: `72px` }}>Salut Gatsby!</div>
)
```

❌ DON'T:

```js
// Exemple
import React from "react"
export default () => (
  // 'purple' is a CSS keyword
  <div style={{ color: `morado`, fontSize: `72px` }}>Salut Gatsby!</div>
)
```

❌ DEFINITELY DON'T:

```js
import React from "react"
export default () => (
   <div style = {{color: `morado`, fontSize:` 72px`}}> Salut Gatsby! </div>
)
```

### External Links

If an external link is to an article in a reference like [MDN] or [Wikipedia], and a version of that article exists in your language that is of decent quality, consider linking to that version instead.

[mdn]: https://developer.mozilla.org/fr/
[wikipedia]: https://fr.wikipedia.org/wiki/Wikip%C3%A9dia:Accueil_principal

Exemple:

```md
React elements are [immutable](https://en.wikipedia.org/wiki/Immutable_object).
```

✅ OK:

```md
Les éléments de React sont [immuable](https://fr.wikipedia.org/wiki/Objet_immuable).
```

For links that have no equivalent (Stack Overflow, YouTube videos, etc.), just use the English link.

## Glossary

Use this section to list how common technical terminology should be translated.

| Term   | Translation |
| ------ | ----------- |
| Plugin | ??          |
| Theme  | ??          |
| Query  | ??          |
