## ▶ Rendering in another place — `<teleport to="...">`

### What is that?

By default, every component is rendered inside the same part of the DOM where it's declared in the template.<br>
However, sometimes you want a component to be rendered elsewhere in the DOM, even though it is declared inside another component.<br>
That's what `teleport` does.<br>
It moves the actual rendered HTML to another place, like directly inside the `<body>` or any specific DOM element.

### When to use it?

Use `teleport` when you need the content to appear in front of everything or outside of restricted containers.<br>
<br>
Common use cases:
* Modal windows
* Tooltips
* Dropdowns
* Sidebars
* Notifications
* Loaders / Spinners
* Context menus (right-click)
* Floating buttons (like "Back to Top" button)
* Anything affected by CSS like `z-index`, `overflow:hidden`, `position: relative`

This avoids layout issues where a component is visually hidden or clipped because of its parent's CSS rules.

### How to use it?

```html
<!-- index.html -->

<body>
  <div id="app"></div>
  <div id="modals"></div> <!-- Target where the modals will be teleported -->
</body>

<!-- Component.vue -->

<template>
  <teleport to="#modals" v-if="showModalOne">
    <Modal />
  </teleport>

  <teleport to="#modals" v-if="showModalTwo">
    <Modal />
  </teleport>

  <!--
    Although `<Modal />` is written inside the component, its HTML is rendered inside `<div id="modals">`.
    Completely outside the regular DOM flow of the component.

    That's why the modal won't be affected by parent elements with overflow: `hidden`, `position: relative` or `z-index` issues.
  -->
</template>

<script setup>
import Modal from './Modal.vue'
import { ref } from 'vue'

const showModalOne = ref(false)
const showModalTwo = ref(false)
</script>
```
