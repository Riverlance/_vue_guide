## ▶ Page Routing — `<router-link>`

### What is it?

Routing is what makes your app behave like a Website with multiple pages, even though it is a Single-Page Application (SPA).<br>
With Vue Router (`vue-router`), you can load different components based on the URL path, like `/`, `/about` or `/contact`.

> [!important]
> If you use Nuxt.js, you don't need to install Vue Router manually.<br>
> Nuxt has built-in routing based on your `pages/` folder, and it lazy-loads all routes automatically by default.<br>
> Nuxt 3 also includes support for Vite, making everything faster and easier to manage.

### When to use it?

✔ Use routing when:

* You want to create navigation between different views or pages;
* You want a clean and readable URL (like `/about`, `/contact`, `/profile` or `/settings`);
* You want dynamic routes like `/user/:id` (which would produce like `/user/7018`);
* You want to lazy-load components to improve performance;
* You want to protect routes (authentication, admin access, etc);
* You want to control navigation history.

❌ When to avoid disabling lazy-loading:
* If your application is large or has many routes, disabling lazy-loading can slow down the initial load.
* In public applications, where time to first render matters, lazy-loading improves initial performance.

Lazy-loading is almost always a good practice, especially in large SPAs.<br>
But in small projects, internal applications or critical real-time applications, disabling lazy-loading can be beneficial.

### How to use it?

1. Install `vue-router` (if not already)

```bash
npm install vue-router
```

1. Create your router file

```js
// src/router/index.js

import { createRouter, createWebHistory } from 'vue-router'
import HomeView from '../views/HomeView.vue'
import AboutView from '@/views/AboutView.vue'
import JobsView from '@/views/jobs/JobsView.vue'
import JobDetailsView from '@/views/jobs/JobDetailsView.vue'
import NotFoundView from '@/views/NotFoundView.vue'

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
  },

  {
    path: '/jobs',
    name: 'jobs',
    component: JobsView
  },

  {
    path: '/jobs/:id',
    name: 'job_details',
    props: true,
    component: JobDetailsView
  },

  // Redirect (eg, when you deprecate the page `/all-jobs` and you want for users to be redirected to `/jobs`)
  { path: '/all-jobs', redirect: '/jobs' },

  // Catch-all 404
  // This code creates a catch-all route that matches any path not defined in the router.
  // It shows the NotFoundView component when a user visits a non-existent page, acting as a custom 404 page.
  {
    path: '/:catchAll(.*)',
    name: 'not_found',
    component: NotFoundView
  },
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
    <!-- By path -->
    <!-- <router-link to="/">Home</router-link> | -->
    <!-- <router-link to="/about">About</router-link> -->

    <!-- By route name -->
    <router-link :to="{ name: 'home' }">Home</router-link> |
    <router-link :to="{ name: 'about' }">About</router-link>
  </nav>

  <button @click="redirect">Redirect</button>
  <button @click="back">Go back</button>
  <button @click="forward">Go forward</button>

  <router-view /> <!-- The current page content will be injected here -->
</template>

<script>
export default {
  methods: {
    redirect() { this.$router.push({ name: 'home' }) /* Go to 'home' page */ },
    back()     { this.$router.back() /* this.$router.go(-1) */ /* Alias */ },
    forward()  { this.$router.forward() /* this.$router.go(1) */ /* Alias */ }
  }
}
</script>

<!-- JobsView.vue -->

<template>
  <h1>Jobs</h1>
  <div v-for="job in jobs" :key="job.id">
    <router-link :to="{ name: 'job_details', params: { id: job.id } }">
      <h2>{{ job.title }}</h2>
    </router-link>
  </div>
</template>

<script>
export default {
  data() {
    return {
      jobs: [
        { id: 1, title: 'Ninja UX Designer', details: 'Lorem ipsum dolor sit amet.'},
        { id: 2, title: 'Ninja Web Developer', details: 'Lorem, ipsum dolor sit amet consectetur adipisicing.'},
        { id: 3, title: 'Ninja Vue Developer', details: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. Dolorum, molestiae?'},
      ]
    }
  }
}
</script>

<!-- JobDetailsView.vue -->

<template>
  <h1>Job Details Page</h1>
  <p>The job id is {{ id }}</p>
</template>

<script>
export default {
  // Without "props: true" in the route
  // data() { return { id: this.$route.params.id } }

  // With "props: true" in the route
  props: [ 'id' ],
}
</script>

<!-- NotFoundView.vue -->

<template>
  <h2>404</h2>
  <h3>Page not found.</h3>
</template>
```
