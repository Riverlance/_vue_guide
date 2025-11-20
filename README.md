# Vue 3 guide

> [!tip]
  **Docs**: https://vuejs.org/guide/introduction.html

---
---
---

<!--
TODO:

* Suspense Component for Spinners / Loaders
-->

# 💻 Using Vue as a component in a Website

This means, you can have your normal Website and inject Vue on it.<br>
To use as component, we will learn the basics only.

> [!important]
  Install the VS Code extension called "Live Server" of Ritwick Dey.<br>
  Right-click on the text area of the `index.html` file, then choose `Open with Live Server` (or `Alt+L Alt+O`) to start the Website preview.<br>
  Choose `Stop Live Server` (or `Alt+L Alt+C`) to stop the Website preview.<br>
  This is not necessary actually, but this is needed in here to simulate applications like Apache, in which will make your Website to be online.

> [!tip]
  If you need more information about how to use it as a component, check the following repository:<br>
  https://github.com/Riverlance/vue-sample-component/commits/main/

1. `[Add vue.global.js]` Use the following script in the HTML file, on the bottom before `</body>`.
```html
<!-- For developing - Useful messages for debugging, detailed warnings, a bit slower, higher size -->
<script src="https://unpkg.com/vue@3.0.2/dist/vue.global.js"></script>

<!-- For production -->
<!-- <script src="https://unpkg.com/vue@3.0.2/dist/vue.global.prod.js"></script> -->
```

> [!tip]
> Use `vue@next` instead of `vue@3.0.2` to get the latest version.

2. `[Add app.js]` Create a script file called `app.js` to init the Vue.
```js
const app = Vue.createApp({
  template: '<h2>I\'m the template.</h2>'
})

app.mount('#app')
```

Add it to the HTML file, on the bottom before `</body>` and after the `vue.global.js`.
```html
<script src="app.js"></script>
```

3. `[div "app"]` Choose where you want to place the Vue component in the HTML file.<br>
  Vue will overwrite the content of this div, which has the id `app`.
```html
<div id="app">
  <h2>This is the default content, if nothing is overwritten by Vue.</h2>
</div>
```

4. `[data()]` You can use it to create reactive state, like a data object, in the Vue component.
```js
const app = Vue.createApp({
  data() {
    return {
      title: 'The Final Empire',
      author: 'Brandon Sanderson',
      age: 45,
    }
  }
})
```
```html
<div id="app">
  <h2>{{ title }}</h2>
  <p>{{ author }}, {{ age }}</p>
</div>
```

5. `[@ instead of v-on]` You can add click events by using `v-on:click` or simply by `@click`, not only on buttons, but on any HTML element.
```html
<div id="app">
  <h2>{{ title }}</h2>
  <p>{{ author }}, {{ age }}</p>

  <button v-on:click="age++">Increase age</button>
  <button @click="age--">Decrease age</button>
  <div @click="title = 'Something else'">Click this text to update the title</div>
</div>
```

6. `[First method]` For more complex methods to be used in click events, do the following.
```js
const app = Vue.createApp({
  data() { /* as mentioned before */ },

  methods: {
    changeTitle(title) { this.title = title },
  },
})
```
```html
<div @click="changeTitle('Words of Radiance')">Click this text to update the title</div>
```

7. `[First button]` Let's create a button to show the main book.
```js
const app = Vue.createApp({
  data() {
    return {
      // as mentioned before
      showMainBook: false,
    }
  },

  methods: {
    // as mentioned before
    toggleShowMainBook() { this.showMainBook = !this.showMainBook },
  },
})
```
```html
<div id="app">
  <div v-if="showMainBook">
    <h2>{{ title }}</h2>
    <!-- as mentioned before (author, age, increase/decrease age, changeTitle) -->
  </div>

  <button @click="toggleShowMainBook">
    <span v-if="showMainBook">Hide main book</span>
    <span v-if="!showMainBook">Show main book</span> <!-- or simply: <span v-else>Show main book</span> -->
  </button>
</div>
```

8. [`v-show`] You could do the same as `v-if` with `v-show` instead, but with a subtle difference.<br>
  The `v-show` div below is hidden (using css `display: none;` on its div) once `showMainBook` is `false`, instead of removing the div block completely as happens with `v-if`.<br>
  This means `v-show` is faster than `v-if`, meaning its better for content that needs to show/hide very often.<br>
  But remember that the div is not completely removed, but hidden instead.
```html
<div v-show="showMainBook">Currently showing books</div>
```

9. `[Mouse events]` Let's create some mouse events.
```js
const app = Vue.createApp({
  data() {
    return {
      // as mentioned before
      x: 0,
      y: 0,
    }
  },

  methods: {
    // as mentioned before
    handleEvent(e, data) {
      console.log(e.type, e)
      if (data) {
        console.log(data)
      }
    },
    handleMousemove(e) {
      this.x = e.offsetX
      this.y = e.offsetY
    },
  },
})
```
```html
<div id="app">
  <!-- as mentioned before -->
  <span @mouseover="handleEvent($event, 7)">Enter</span>
  <span @mouseleave="handleEvent">Leave</span>
  <span @dblclick="handleEvent">Double click</span>
  <span @mousemove="handleMousemove">Mouse move (x{{x}}, y{{y}})</span>
</div>
```

10.  `[v-for]`
```js
const app = Vue.createApp({
  data() {
    return {
      // as mentioned before
      books: [
        { title: 'Name of the Wind', author: 'Patrick Rothfuss', isFavorite: true },
        { title: 'The Way of Kings', author: 'Brandon Sanderson', isFavorite: false },
        { title: 'The Final Empire', author: 'Brandon Sanderson', isFavorite: false },
      ],
    }
  },

  methods: {
    // as mentioned before
  },
})
```
```html
<div id="app">
  <!-- as mentioned before -->
  <h2>Books</h2>
  <ul>
    <li v-for="book in books">
      <h2>{{ book.title }}</h2>
      <p>{{ book.author }}</p>
    </li>
  </ul>
</div>
```

11. `[:TAGATTRIBUTE instead of v-bind:TAGATTRIBUTE]` Attribute binding is used so that the value is evaluated as a reactive JavaScript expression.<br>
  `src="image.filepath"` is just a literal string and will not be interpreted as a variable.<br>
  But `:src="image.filepath"` is evaluated as a reactive JavaScript expression.
```js
const app = Vue.createApp({
  data() {
    return {
      // as mentioned before
      url: '//google.com',
    }
  },

  methods: {
    // as mentioned before
  },
})
```
```html
<div id="app">
  <!-- as mentioned before -->
  <a v-bind:href="url" target="_blank">Google Website</a><br>
  <a :href="url" target="_blank">Google Website</a>
</div>
```

12. `[Computed values]` Reactive values that are automatically updated when their dependencies change.<br>
  It's ideal for filtering or transforming data based on the component's state, without repeating logic inside the template.<br>
  Unlike methods, computed properties are cached and only re-evaluated when needed, which improves performance and keeps your code clean and efficient.
```js
const app = Vue.createApp({
  data()   { /* as mentioned before */ },
  methods: { /* as mentioned before */ },

  computed: {
    filteredBooks() {
      return this.books.filter((book) => book.isFav)
    }
  },
})
```
```html
<div id="app">
  <!-- as mentioned before -->
  <h2>Favorite books</h2>
  <ul>
    <li v-for="book in filteredBooks">
      <h2>{{ book.title }}</h2>
      <p>{{ book.author }}</p>

      <!-- Dynamic classes -->
      <span class="book" :class="book.isFavorite ? 'fav' : ''">Lorem ipsum</span>
      <span class="book" :class="[book.isFavorite ? 'fav' : '', isSelected ? 'sel' : '']">Lorem ipsum</span>
      <span class="book" :class="[book.isFavorite ? 'fav' : '']">Lorem ipsum</span>
      <span class="book" :class="{ fav: book.isFavorite, sel: isSelected }">Lorem ipsum</span>
      <span class="book" :class="{ fav: book.isFavorite }">Lorem ipsum</span>
    </li>
  </ul>
</div>
```

---
---
---

# 💻 Using Vue

* Installs all project dependencies.
```
npm install
```

* Compiles and hot-reloads for development. (*1)
```
npm run serve
```

* Compiles and minifies for production. (*1)
```
npm run build
```

> [!important]
> Read all `.md` files in this repository to learn.

> [!note]
> (*1) You can find this script type in `package.json`.

> [!tip]
> You can create a global .css file, like at `src/assets/css/global.css`.<br>
> You can include it globally by importing in `src/main.js` as `import './assets/css/global.css'`.

---

## Importing

Use `.` for relative imports.<br>
Use `@` for absolute imports: `@` == `src`.

```html
<template>
  <img src="./assets/logo.png">
  <img src="@/assets/logo.png">
</template>

<script>
import ChildDot from './components/Child.vue'
import ChildAt from '@/components/Child.vue'

export default {
  components: { ChildDot, ChildAt }
}
</script>

<style scoped>
@import "./styles/main.css";
@import "@/styles/main.css";

.bg-dot {
  background: url("./assets/bg.png");
}
.bg-at {
  background: url("@/assets/bg.png");
}
</style>
```
