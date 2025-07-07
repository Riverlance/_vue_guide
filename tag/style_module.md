## ▶ Module style — `<style module>`

### What is it?

The `module` attribute makes CSS classes unique for each component.<br>
It prevents styles with the same name from mixing between different components.<br>
It isolate class names in JavaScript.

> [!important]
> If you will use a CSS framework like Tailwind, you will only need this to make exceptions or fixes.<br>
> You can also have two separate `<style>` blocks, one scoped and one module scoped, although it is rare to need both simultaneously.

### When to use it?

Use `<style module>` when you want to generate unique class names and avoid any style conflicts between components.

### How to use it?

```html
<!-- Component1.vue -->

<template>
  <p :class="$style.title">Hello!</p> <!-- Will be affected by the green color -->
</template>

<style module>
  .title { color: green; }
</style>

<!-- Component2.vue -->

<template>
  <p :class="$style.title">Hello!</p> <!-- Will NOT be affected by Component1 styles -->
</template>

<style module>
  .title { color: blue; }
</style>
```

Each component has its own version of the `.title` class with a unique name like `.title__abc123`, so the styles won't conflict, even if the class names are the same.

> [!tip]
> You can still apply global styles with `:global(...)`:
```html
<style module>
  .title { color: green; } /* Scoped to this component only */
  :global(body) { background: #eee; } /* Affects all components */
</style>
```
