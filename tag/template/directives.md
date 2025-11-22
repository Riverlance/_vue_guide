## ▶ Directives

### `v-bind`

`[:TAGATTRIBUTE instead of v-bind:TAGATTRIBUTE]` Attribute binding is used so that the value is evaluated as a reactive JavaScript expression.<br>
`src="image.filepath"` is just a literal string and will not be interpreted as a variable.<br>
But `:src="image.filepath"` is evaluated as a reactive JavaScript expression.
```html
<div id="app">
  <a v-bind:href="url" target="_blank">Google Website</a><br>
  <a :href="url" target="_blank">Google Website</a>
  <a href="url" target="_blank">Google Website</a> <!-- This is NOT how to use it; It will use `url` as a literal string, instead of a reactive JavaScript expression (which would use `url` as a variable) -->
</div>
```

### `v-if`

The `v-if` directive is used to conditionally show or hide a block of content.

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

### `v-show`

You could do the same as `v-if` with `v-show` instead, but with a subtle difference.<br>
The `v-show` div below is hidden (using css `display: none;` on its div) once `showMainBook` is `false`, instead of removing the div block completely as happens with `v-if`.<br>
This means `v-show` is faster than `v-if`, meaning its better for content that needs to show/hide very often.<br>
But remember that the div is not completely removed, but hidden instead.
```html
<div v-show="showMainBook">Currently showing books</div>
```

### `v-for`

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

### `v-model`

It creates two-way data binding between your template and your JavaScript state.<br>
In a nutshell, it means: Keep this input and this variable always synchronized.<br>
<br>
* When the user changes the input, the variable in your component updates automatically.<br>
* When the variable changes in code, the input also updates automatically.<br>
<br>
It saves you from manually handling @input events and :value bindings.<br>
It keeps your UI and your data in sync with very little code.
```html
<template>
  <input v-model="name" placeholder="Your name" />
  <!-- <input :value="name" @input="name = $event.target.value" placeholder="Your name" /> --> <!-- This is the same as the above line -->

  <p>Hello, {{ name }}!</p>
</template>

<script setup>
import { ref } from 'vue'

const name = ref('')
</script>
```

A complex example - multiple checkboxes:
```html
<template>
  <form>
    <label>Names:</label>
    <div>
      <input type="checkbox" value="river" v-model="names" />
      <label>River</label>
    </div>
    <div>
      <input type="checkbox" value="mario" v-model="names" />
      <label>Mario</label>
    </div>
    <div>
      <input type="checkbox" value="yoshi" v-model="names" />
      <label>Yoshi</label>
    </div>
  </form>

  <p>Names: {{ names }}</p> <!-- ['river', 'mario', 'yoshi'] if all checkboxes are checked -->
</template>

<script setup>
import { ref } from 'vue'

const names = ref([])
</script>
```

Another complex example - multiple inputs with a comma-separated list of skills:
```html
<template>
  <form>
    <label>Skills (press Alt+, to add):</label>
    <input type="text" v-model="tempSkill" @keyup.alt="addSkill">

    <div v-for="skill in skills" :key="skill" class="pill">
      <span @click="deleteSkill(skill)">{{ skill }}</span>
    </div>
  </form>

  <p>Skills: {{ skills }}</p>
</template>

<script setup>
import { ref } from 'vue'

const tempSkill = ref('')
const skills    = ref([])

const addSkill = (e) => {
  if (e.key === ',' && tempSkill.value) {
    if (!skills.value.includes(tempSkill.value))
      skills.value.push(tempSkill.value)
    tempSkill.value = ''
  }
}

const deleteSkill = (skill) => {
  skills.value = skills.value.filter((item) => {
    return item !== skill
  })
}
</script>
```

`v-model` isn't just for inputs.<br>
It works in any component (including components you create) as long as the component implements the `v-model` interface (`modelValue` + `update:modelValue`).
```html
<!-- Parent.vue -->

<Toggle v-model="enabled" />
<p>Enabled: {{ enabled }}</p>

<!-- Toggle.vue -->

<template>
  <div @click="toggle">
    {{ modelValue ? 'ON' : 'OFF' }} <!-- The value is bound to the `modelValue` prop -->
  </div>
</template>

<script setup>
const props = defineProps(['modelValue'])
const emit = defineEmits(['update:modelValue'])

const toggle = () => emit('update:modelValue', !props.modelValue) // The value is emitted to the `modelValue` prop
</script>
```
