## ▶ Page Routing — `<router-link>`

### What is it?

Routing is what makes your app behave like a Website with multiple pages, even though it is a Single-Page Application (SPA).<br>
With Vue Router (`vue-router`), you can load different components based on the URL path, like `/`, `/about` or `/contact`.

> [!important]
> If you use Nuxt.js, you don't need to install Vue Router manually.<br>
> Nuxt has built-in routing based on your `pages/` folder, and it lazy-loads all routes automatically by default.<br>
> Nuxt 3 also includes support for Vite, making everything faster and easier to manage.

### When to use it?

Use routing when:

* You want to create navigation between different views or pages;
* You want a clean and readable URL (like `/about`, `/contact`, `/profile` or `/settings`);
* You want dynamic routes like `/user/:id` (which would produce like `/user/7018`);
* You want to lazy-load components to improve performance;
* You want to protect routes (authentication, admin access, etc);
* You want to control navigation history.

### How to use it?

1. Install `vue-router` (if not already)

```bash
npm install vue-router
```

2. Create your router file (`src/router/index.js`)

```js
// src/router/index.js

import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'

const routes = [
  {
    path: '/',
    name: 'home',
    component: HomeView
  },
  {
    path: '/about',
    name: 'about',

    // Lazy-loading
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/AboutView.vue') // Lazy-loaded
    // This `import()` is asynchronous: it only downloads the file when needed.
    // This breaks your code into "smaller pieces" (called chunks), which the browser downloads on demand.
  }
]

const router = createRouter({
  history: createWebHistory(process.env.BASE_URL), // For Vue CLI
  // history: createWebHistory(import.meta.env.BASE_URL), // For Vite
  routes
})

export default router
```

3. Add `<router-view>` to your app with `<router-link>`

```html
<!-- App.vue -->

<template>
  <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav>
  <router-view /> <!-- The current page content will be injected here -->
</template>
```
