## ▶ Reusable logic — Composable

### What is it?

In the **Composition API**, a **composable** is a function that encapsulates **reactive state and logic**, so it can be reused across multiple components.<br>
It's like a small library of functions that you can import and use in your components of Composition API.<br>
It usually returns reactive variables (`ref`, `reactive`) and functions to interact with them.

- Composables help **separate concerns** and keep components clean.
- They **start with a function** that can return reactive state and methods.
- They allow **code reuse** across components without duplicating logic.
- They can be imported and used in multiple components just like regular functions.

> [!note]
  Composables are a feature of the Composition API.<br>
  There is no direct equivalent in the Options API.

> [!tip]
  `fetch` has a second parameter, which is an object that contains the `method` and `headers` properties.<br>
  **Examples**:<br>
  ▶ ADD: `fetch('http://localhost:3000/jobs', { method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(newJob) }).then(() => emit('added', job.id)).catch(err => console.log(err))`<br>
  ▶ EDIT PART: `fetch('http://localhost:3000/jobs/1', { method: 'PATCH', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ status: !oldStatus }) }).then(() => emit('complete', job.id)).catch(err => console.log(err))`<br>
  ▶ OVERWRITE: `fetch('http://localhost:3000/jobs/1', { method: 'PUT', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(editedJob) }).then(() => emit('updated', job.id)).catch(err => console.log(err))`<br>
  ▶ DELETE: `fetch('http://localhost:3000/jobs/1', { method: 'DELETE' }).then(() => emit('deleted', job.id)).catch(err => console.log(err))`

> [!tip]
  This content is related to `ref()`, `computed()`, `watch()`, and `setup()`.

### When to use it?

Use it to organize and encapsulate application JavaScript logic into reusable files, like a small library of functions.

### How to use it?

```js
// myWebsite.js

import { ref } from 'vue'

const usePosts = () => {
  const posts = ref([])
  const error = ref(null)

  const load = async () => {
    try {
      let data = await fetch('//localhost:3000/posts')
      if (!data.ok) throw Error('No data available.')
      posts.value = await data.json()
    } catch(err) {
      error.value = err.message
    }
  }

  return { posts, error, load }
}

export { usePosts } // Export multiple functions
```

```html
<!-- HomeView.vue -->

<template>
  <div>
    <PostListView v-if="posts.length" :posts="posts" />
    <div v-else>
      <p v-if="error">{{ error }}</p>
      <p v-else>Loading...</p>
    </div>
  </div>
</template>

<script setup>
import { usePosts } from '@/composables/myWebsite' // Import multiple functions
// import * as postsAPI from '@/composables/myWebsite' // Import multiple functions; Another option (e.g, `postsAPI.usePosts()`)
import PostListView from '@/components/PostListView.vue'

const { posts, error, load } = usePosts()
load()
</script>
```

If you want a composable with a single function, you can do something like this:
```js
// myWebsite.js

import { ref } from 'vue'

const usePosts = () => {
  // ... same content
}

export default usePosts // Export a single function
```

```html
<!-- HomeView.vue -->

<template>
  <!-- Same content -->
</template>

<script setup>
import usePosts from '@/composables/myWebsite' // Import a single function
import PostListView from '@/components/PostListView.vue'

const { posts, error, load } = usePosts()
load()
</script>
```

Simplified (best) way:

```js
// myWebsite.js

import { ref } from 'vue'

/**
 * Fetches posts from an API.
 *
 * @example
 * const { posts, error, load } = usePosts()
 * load()
 */
export function usePosts() { ... } // Export a single function
export async function useSubmit() { ... } // Export another function
```

```html
<!-- HomeView.vue -->

<template>
  <!-- Same content -->
</template>

<script setup>
import * as myWebsite from '@/composables/myWebsite' // Import multiple functions
// import { usePosts } from '@/composables/myWebsite' // Import multiple functions // You still can do this
import PostListView from '@/components/PostListView.vue'

const { posts, error, load } = myWebsite.usePosts()
load()
</script>
```

For a default function:
```js
// myWebsite.js
export default function usePosts() { ... } // Export default function
```
```html
<!-- HomeView.vue -->

<template>
  <!-- Same content -->
</template>

<script setup>
import usePosts from '@/composables/myWebsite' // Import default function
import PostListView from '@/components/PostListView.vue'

const { posts, error, load } = usePosts()
load()
</script>
```

> [!warning]
  Avoid using `export const foo = () => { ... }` (use `export function foo() { ... }` instead) because it loses hoisting and the function name becomes `undefined` until it is declared.<br>
  Example:<br>
  usePosts() // Error: Cannot access before initialization<br>
  export const usePosts = () => { ... }

---

### POST request example

```js
// Composable
// myWebsite.js
import { ref } from 'vue'

export function useSubmit() {
  const loading = ref(false)
  const error = ref(null)

  const handleSubmit = async (payload) => {
    loading.value = true
    error.value = null
    try {
      const res = await fetch('/api/submit', {
        method: 'POST',
        body: JSON.stringify(payload)
      })
      return await res.json()
    } catch (err) {
      error.value = err
    } finally {
      loading.value = false
    }
  }

  return { handleSubmit, loading, error }
}
```
```html
<!-- HomeView.vue -->

<script setup>
import { useSubmit } from '@/composables/myWebsite'

const { handleSubmit, loading, error } = useSubmit()
await handleSubmit({ name: 'Gabriel' })
</script>
```

---

### Documentation

```js
/**
 * Prints the first parameter.
 *
 * @param {any} firstParam The first parameter.
 * @returns {boolean} `true` as always, `false` as never.
 * @example
 *
 * ### Example
 *
 * ```js
 * test(7)
 * // 7
 *
 * console.log(test(7))
 * // 7
 * // true
 * ```
 */
export function test(firstParam) {
  console.log(firstParam)
  return true
}
```
