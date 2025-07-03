## ▶ Simplified Composition API — `<script setup>`

### What is that?

The `setup` attribute activates the Composition API in a simplified way.<br>
It removes the need for `export default`, the `setup()` function or manually returning variables.<br>
Everything declared inside is automatically available in the `<template>`.

> [!important]
> It helps reduce repeated code and makes components easier to understand and create.

### When to use it?

Use `<script setup>` when you want to write cleaner, faster and more modern components using the Composition API.

### How to use it?

```html
<template>
  <p>{{ message }}</p> <!-- Will display the message from the script -->
</template>

<script setup>
const message = 'Hello from setup!'
</script>
```

Everything inside the `<script setup>` block is scoped to this component and is automatically exposed to the template — no need for `return` or `this`.

> [!tip]
> If you use TypeScript, just add lang="ts":
```html
<script setup lang="ts">
  const count: number = 10
</script>
```
