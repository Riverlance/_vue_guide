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

2. `[data()]` You can use it to create reactive state, like a data object, in the Vue component.
```js
// app.js
const app = Vue.createApp({
  // With 'template' attribute, we can use it to define the template.
  // Or we can use the 'data()' attribute to define the template.
  // template: '<h2>I\'m the template.</h2>'

  data() {
    return {
      title: 'The Final Empire',
      author: 'Brandon Sanderson',
      age: 45,
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

app.mount('#app')
```
```html
<!-- index.html -->
<div id="app">
  <h2>{{ title }}</h2>
  <p>{{ author }}, {{ age }}</p>

  <span @mouseover="handleEvent($event, 7)">Enter</span>
  <span @mouseleave="handleEvent">Leave</span>
  <span @dblclick="handleEvent">Double click</span>
  <span @mousemove="handleMousemove">Mouse move (x{{x}}, y{{y}})</span>
</div>
```

Add it to the HTML file, on the bottom before `</body>` and after the `vue.global.js`.
```html
<script src="app.js"></script>
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
