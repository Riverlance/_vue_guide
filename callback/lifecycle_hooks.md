## ▶ Lifecycle Hooks

### What is that?

Lifecycle hooks are special functions that are called at different stages of a component's life:
* Create — When it is created (*1);
* Add — When it is added to the DOM;
* Update — When it updates;
* Remove — And when it is removed.

**Lifecycle Flow:**

| Phase             | Hook              |
|-------------------|-------------------|
| 1. Creation (*1)  | `beforeCreate` → `created` |
| 2. Mounting       | `beforeMount` → `mounted` |
| 3. Updating       | `beforeUpdate` → `updated` |
| 4. Unmounting     | `beforeUnmount` → `unmounted` |

Lifecycle hooks are helpful anytime you want to interact with the DOM, the browser or an external library.<br>
You can only use lifecycle hooks inside components, but not in plain JavaScript files.

> [!note]
> (*1) These (`beforeCreate` and `created`) are not used with the simplified Composition API (`<script setup>`), because the code inside `<script setup>` runs in between them.<br>
> So the `<script setup>` block already replaces their purpose.

### When to use it?

Common usages:
* Start or stop an interval or timer;
* Add event listeners (like window.addEventListener);
* Fetch data from an API when the component is mounted;
* Listen to keyboard or mouse events;
* Use console.log() to debug lifecycle;
* Focus an input when the component is shown;
* Track or reset animations or transitions;
* Clean up before unmounting (like clearInterval or removeEventListener);
* Manually update a third-party library (e.g. Chart.js, Leaflet, Swiper);
* Scroll to a specific element after DOM is loaded;
* Log analytics when the user views a component;
* Watch how the component changes over time;
* Do something before the component is destroyed.

### How to use it?

* `onBeforeMount()`

```html
<script setup>
import { onBeforeMount } from 'vue'
onBeforeMount(() => { console.log('Component will be mounted soon.') })
</script>
```

* `onMounted()`

```html
<script setup>
import { onMounted } from 'vue'
onMounted(() => { console.log('Component is now mounted.') })
</script>
```

* `onBeforeUpdate()`

```html
<script setup>
import { onBeforeUpdate } from 'vue'
onBeforeUpdate(() => { console.log('Component will update soon.') })
</script>
```

* `onUpdated()`

```html
<script setup>
import { onUpdated } from 'vue'
onUpdated(() => { console.log('Component has been updated.') })
</script>
```

* `onBeforeUnmount()`

```html
<script setup>
import { onBeforeUnmount } from 'vue'
onBeforeUnmount(() => { console.log('Component will be unmounted.') })
</script>
```

* `onUnmounted()`

```html
<script setup>
import { onUnmounted } from 'vue'
onUnmounted(() => { console.log('Component is now unmounted.') })
</script>
```

#### Practical example

This component example starts a counter when the component is mounted and stops it when the component is unmounted.

```html
<template>
  <p>{{ count }}</p>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const count  = ref(0)
let interval = null

onMounted(() => {
  interval = setInterval(() => {
    count.value++
  }, 1000)
})

onUnmounted(() => {
  clearInterval(interval)
})
</script>
```
