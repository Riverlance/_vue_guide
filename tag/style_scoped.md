## ▶ Scoped style — `<style scoped>`

### What is that?

The `scoped` attribute ensures that the styles apply only to this component, not globally.<br>
It isolate styles in the component DOM.

> [!important]
> If you will use a CSS framework like Tailwind, you will only need this to make exceptions or fixes.<br>
> You can also have two separate `<style>` blocks, one scoped and one module scoped, although it is rare to need both simultaneously.

### When to use it?

Use `<style scoped>` when you want your styles to apply only to the current component without affecting others.

### How to use it?

```html
<!-- Component1.vue -->

<template>
  <p>Hello!</p> <!-- Will be affected by the red color -->
</template>

<style scoped>
  p { color: red; }
</style>

<!-- Component2.vue -->

<template>
  <p>Hello!</p> <!-- Will NOT be affected by the red color -->
</template>
```

The `<p>` tag inside this component will be red, but `<p>` tags elsewhere won't be affected.

> [!tip]
> You can still apply global styles with `:global(...)`:
```html
<style scoped>
  p { color: red; } /* Scoped to this component only */
  :global(body) { background: #eee; } /* Affects all components */
</style>
```
