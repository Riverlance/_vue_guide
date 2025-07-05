## ▶ Exposing to parent — `defineExpose()`

### What is that?

Inside a `<script setup>` block, everything you declare is automatically available to the `<template>` of the same component.<br>
However, none of those variables or functions are exposed to the parent component by default — even if the parent uses `ref` to access the child.<br>
To explicitly expose something to the outside (parent), you need to use `defineExpose()`.

> [!tip]
> This content is related to `<script setup>`.

### When to use it?

Use defineExpose() when you want the parent component to be able to call a method; or access a variable of the child component (avoid this; it breaks the reactive flow).

### How to use it?

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
