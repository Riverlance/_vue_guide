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
  <p v-if="isMessageVisible">{{ message }} - {{ number }}</p>
  <button @click="toggleMessage">Toggle Message</button> <!-- This will hide/show the `<p>` content above -->

  <a :href="url" :target="anotherTab ? '_blank' : '_self'">Click here!</a>
  <a href="//google.com" target="_blank">Click here!</a> <!-- Same as above -->

  <span :class="isFavorite ? 'fav' : ''">Lorem ipsum</span>
  <span :class="[isFavorite ? 'fav' : '', isSelected ? 'sel' : '']">Lorem ipsum</span>
  <span :class="[isFavorite ? 'fav' : '']">Lorem ipsum</span>
  <span :class="{ fav: isFavorite, sel: isSelected }">Lorem ipsum</span>
  <span :class="{ fav: isFavorite }">Lorem ipsum</span>

</template>

<script setup>
  import { ref } from 'vue'

  const isMessageVisible = ref(true)
  function toggleMessage() {
    isMessageVisible = !isMessageVisible
  }

  const message    = 'Hello from setup!'
  const number     = 7
  const url        = '//google.com'
  const anotherTab = true
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
