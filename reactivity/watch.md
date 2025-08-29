## ▶ Reactivity tracking — watch() & watchEffect()

### What is it?

The `watch()` and `watchEffect()` functions are used to **react to changes in reactive data** in the Composition API.<br>
They let you run side effects (like logging, fetching data or stopping a process) whenever certain values change.

- `watch()`: Observes one or more specific sources, runs **only when they change**.
- `watchEffect()`: Runs immediately and automatically tracks any reactive values used inside its callback.

> [!tip]
> This content is related to `ref()`, `computed()` and lifecycle hooks.

### When to use it?

* Use `watch()` when you want to track a **specific source** (like a ref or computed) and only react when that source changes.<br>
* Use `watchEffect()` when you want to **auto-track multiple reactive dependencies** inside the function or need the effect to run immediately on startup.

### How to use it?

* Basic usage of `watch()`

```html
<script setup>
import { ref, watch } from 'vue'

const search = ref('')

// Only runs when 'search' changes
watch(search, (newValue, oldValue) => {
  console.log('search changed from', oldValue, 'to', newValue)
})
</script>
```

* Basic usage of `watchEffect()`

```html
<script setup>
import { ref, watchEffect } from 'vue'

const search = ref('')

// Runs once immediately, then on every change of 'search'
watchEffect(() => {
  console.log('search is now:', search.value)
})
</script>
```

* Example: tracking changes and stopping watchers

```html
<template>
  <div>
    <input type="text" v-model="search">
    <p>Search term: {{ search }}</p>

    <div v-for="name in matchingNames" :key="name">
      {{ name }}
    </div>

    <button @click="handleClick">Stop Watching</button>
  </div>
</template>

<script setup>
import { ref, computed, watch, watchEffect } from 'vue'

const search = ref('')
const names = ref(['Mario', 'Yoshi', 'Luigi', 'Toad', 'Bowser', 'Koopa', 'Peach'])

// watch → tracks only 'search'
const stopWatchCallback = watch(search, () => {
  console.log('watch function ran')
})

// watchEffect → runs immediately, tracks everything inside
const stopWatchEffectCallback = watchEffect(() => {
  console.log('watchEffect function ran', search.value)
})

const matchingNames = computed(() => {
  return names.value.filter((name) =>
    name.toLowerCase().includes(search.value.toLowerCase())
  )
})

const handleClick = () => {
  stopWatchCallback()       // stop watch()
  stopWatchEffectCallback() // stop watchEffect()
}
</script>
```

* Old way (Options API)

```html
<script>
export default {
  data() { return { search: '' } },

  watch: {
    search(newVal, oldVal) {
      console.log('search changed from', oldVal, 'to', newVal)
    }
  }
}
</script>
```

> [!important]
  The **Options API** does not provide `watchEffect`. This feature is exclusive to the **Composition API**, since it relies on automatic dependency tracking inside `setup()`. In the **Options API**, you would need to explicitly declare dependencies using `watch` or place logic inside lifecycle hooks like `created()` or `mounted()`.

Here’s a short example showing how you might replicate `watchEffect` in **Options API** using `watch` + `mounted`:

```html
<script>
export default {
  data() { return { search: '' } },

  mounted() {
    // Run immediately (similar to watchEffect's first run)
    console.log('search is now:', this.search)
  },

  watch: {
    // Runs on every change (like watchEffect's reactivity tracking)
    search(newVal) {
      console.log('search is now:', newVal)
    }
  }
}
</script>
```
