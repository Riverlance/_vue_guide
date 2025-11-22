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
// postsComposable.js

import { ref } from 'vue'

const getPosts = () => {
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

export { getPosts }
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
import { getPosts } from '@/composables/postsComposable'
import PostListView from '@/components/PostListView.vue'

const { posts, error, load } = getPosts()
load()
</script>
```
