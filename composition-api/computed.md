## ▶ Reactive values — computed()

### What is it?

The `computed()` function lets you create reactive values that are **derived from other reactive data**.<br>
It automatically updates whenever the values it depends on change.<br>
Think of it as a *smart variable* that recalculates itself only when needed.

> [!tip]
> This content is related to `ref()`/`reactive()` and `watch()`.

### When to use it?

Use `computed()` when you need to **transform, filter or combine existing reactive data** and always want the result to stay updated automatically.

Examples:
- Filtering a list
- Formatting text
- Creating dynamic values based on state

### How to use it?

* Basic usage

```html
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('Mario')
const lastName = ref('Bros')

const fullName = computed(() => `${firstName.value} ${lastName.value}`)
</script>

<template>
  <p>{{ fullName }}</p> <!-- Output: Mario Bros -->
</template>
```

* Example: filtering a list (Composition API)

```html
<template>
  <div>
    <input type="text" v-model="search">
    <p>Search term: {{ search }}</p>

    <div v-for="name in matchingNames" :key="name">
      {{ name }}
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const search = ref('')
const names = ref(['Mario', 'Yoshi', 'Luigi', 'Toad', 'Bowser', 'Koopa', 'Peach'])

const matchingNames = computed(() => {
  return names.value.filter((name) =>
    name.toLowerCase().includes(search.value.toLowerCase())
  )
})
</script>
```

* Old way (Options API)

```html
<script>
export default {
  data() {
    return {
      search: '',
      names: ['Mario', 'Yoshi', 'Luigi', 'Toad', 'Bowser', 'Koopa', 'Peach']
    }
  },
  computed: {
    matchingNames() {
      return this.names.filter((name) =>
        name.toLowerCase().includes(this.search.toLowerCase())
      )
    }
  }
}
</script>
```
