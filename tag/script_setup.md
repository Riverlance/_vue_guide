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

---

### Exposing to parent — `defineExpose()`

Inside a `<script setup>` block, everything you declare is automatically available to the `<template>` of the same component.<br>
However, none of those variables or functions are exposed to the parent component by default — even if the parent uses `ref` to access the child.<br>
To explicitly expose something to the outside (parent), you need to use `defineExpose()`.<br>
<br>
Use defineExpose() when you want the parent component to be able to call a method or access a variable from the child component.

```html
<!-- ChildComponent.vue -->

<template>
  <input v-model="name" />
</template>

<script setup>
  import { ref } from 'vue'

  const name = ref('')
  function resetForm() {
    name.value = ''
  }

  defineExpose({ resetForm }) // Now the parent can call this
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent ref="child" />
  <button @click="childReset">Reset</button>
</template>

<script setup>
import { ref } from 'vue'
import ChildComponent from './ChildComponent.vue'

const child = ref(null)
function childReset() {
  if (child.value)
    child.value.resetForm() // This works because of defineExpose
}
</script>
```
