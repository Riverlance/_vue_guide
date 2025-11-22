## ▶ $refs and ref()

### What is it?

#### `ref()`

Reactive state should be handled with `ref()` for primitives and objects.<br>
You can also use `reactive()` for objects.

#### `$refs`

The `$refs` is used to access DOM elements or component instances directly from JavaScript.<br>
Use `$refs` as a last option when props, emits and reactive bindings don't meet your needs.

> [!warning]
  Relying on `$refs` (instead of `ref()`) to read or manipulate state is discouraged.<br>
  It should be used only when state cannot be managed through Vue's reactivity system, since it breaks the reactive flow.<br>
  e.g., focusing inputs, reading element sizes, manipulating non-Vue libraries or working with canvas.

### When to use it?

#### `ref()`

Use ref() to make any value reactive — it works for both primitives and objects.<br>
It is also the safest option when exposing state from composition functions because individual properties remain reactive.

#### `reactive()`

Use `reactive()` for objects only.<br>
It allows you to mutate the object directly without `.value` (e.g., `obj.name = 'Yoshi` instead of `obj.value.name = 'Yoshi'`).<br>
But there are some drawbacks:<br>
* It cannot be used for primitive values, because they lose reactivity.
* It is not ideal when returning individual properties of an object (instead of the entire object) from a composition function, because destructuring the object can break reactivity.
* `ref()` is more predictable when you need to expose the state.

#### `$refs`

* **Case 1**: To manipulate a DOM element directly (e.g., focus an input, add/remove classes).
* **Case 2**: To call methods on a child component (e.g., open a modal, reset a form).
* **Case 3**: To read internal state of a child component that's not exposed via props or emits.

#### Resume

In short, use:
* `ref()` for primitive values and objects;
* `reactive()` for objects only when you don't want to use `.value`; and
* `$refs` when it is not possible to solve the problem using props, emits or reactive bindings.

### How to use it?

#### `ref()`

```html
<template>
  <p>{{ name }}</p>
  <button @click="handleClick">Update Name</button>
</template>

<script setup>
  import { ref } from 'vue'

  const name = ref('John Doe')

  const handleClick = () => {
    name.value = 'Jane Doe' // Updates on templates because it is reactive (`ref()`)
  }

  return { name, handleClick }
</script>
```

#### `$refs`

* **Case 1** - To manipulate a DOM element directly

Modern way (simplified Composition API):

```html
<template>
  <input ref="name" type="text" placeholder="Your name" />
  <button @click="activate">Activate</button>
</template>

<script setup>
  import { ref } from 'vue'

  const name     = ref(null)
  const activate = () => {
    name.value.classList.add('active') // Add a class using JS
    name.value.focus()                 // Focus the input
  }
</script>
```

Old way:

```html
<template>
  <input ref="name" type="text" placeholder="Your name" />
  <button @click="activate">Activate</button>
</template>

<script>
  export default {
    methods: {
      activate() {
        this.$refs.name.classList.add('active') // Add a class using JS
        this.$refs.name.focus()                 // Focus the input
      }
    }
  }
</script>
```

* **Case 2** - To call methods on a child component

Modern way (simplified Composition API):

```html
<!-- ChildComponent.vue -->

<template>
  <input v-model="name" />
</template>

<script setup>
  import { ref } from 'vue'

  const name      = ref('')
  const resetForm = () => {
    name.value = ''
  }

  defineExpose({ resetForm })
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent ref="child" />
  <button @click="resetChild">Reset Child</button>
</template>

<script setup>
  import { ref }        from 'vue'
  import ChildComponent from './ChildComponent.vue'

  const child      = ref(null)
  const resetChild = () => {
    child.value.resetForm() // Calls method inside child
  }
</script>
```

Old way:

```html
<!-- ChildComponent.vue -->

<template>
  <input v-model="name" />
</template>

<script>
  export default {
    data() {
      return {
        name: ''
      }
    },

    methods: {
      resetForm() {
        this.name = ''
      }
    }
  }
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent ref="child" />
  <button @click="resetChild">Reset Child</button>
</template>

<script>
  import ChildComponent from './ChildComponent.vue'

  export default {
    components: { ChildComponent },
    methods: {
      resetChild() {
        this.$refs.child.resetForm() // Calls method inside child
      }
    }
  }
</script>
```

* **Case 3** - To read internal state of a child component that's not exposed via props or emits

Modern way (simplified Composition API):

```html
<!-- ChildComponent.vue -->

<template>
  <button @click="counter++">Clicked {{ counter }} times</button>
</template>

<script setup>
  import { ref } from 'vue'

  const counter = ref(0)

  defineExpose({ counter })
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent ref="child" />
  <button @click="showInternalState">Show Internal State</button>
</template>

<script setup>
  import { ref }        from 'vue'
  import ChildComponent from './ChildComponent.vue'

  const child = ref(null)

  const showInternalState = () => {
    console.log(child.value.counter) // Access internal reactive variable
  }
</script>
```

Old way:

```html
<!-- ChildComponent.vue -->

<template>
  <button @click="counter++">Clicked {{ counter }} times</button>
</template>

<script>
  export default {
    data() {
      return {
        counter: 0
      }
    }
  }
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent ref="child" />
  <button @click="showInternalState">Show Internal State</button>
</template>

<script>
  import ChildComponent from './ChildComponent.vue'

  export default {
    components: { ChildComponent },
    methods: {
      showInternalState() {
        console.log(this.$refs.child.counter)
      }
    }
  }
</script>
```

### Extra: Refs vs Reactive

#### Reactive doesn't work for primitive values

Reactive doesn't work for primitive values; it expects an object as a parameter.
If you try to update its value later, it won't update on templates.
```js
let name = reactive('Luigi') // Gives a warning (invalid value)
const update = () => { name = 'Yoshi' } // or { name = reactive('Yoshi') } // Doesn't update on templates
return { name, update }
```
```html
<p>{{ name }}</p>
<button @click="update">Update</button>
```

* Correct way for reactive - "Always use the object itself, instead of primitive values or individual properties":
```js
const obj = reactive({ name: 'Luigi' })
const update = () => { obj.name = 'Yoshi' } // Updates on templates
return { obj, update }
```
```html
<p>{{ obj.name }}</p>
<button @click="update">Update</button>
```

* With ref:
```js
const name = ref('Luigi')
const update = () => { name.value = 'Yoshi' } // Updates on templates
return { name, update }
```
```html
<p>{{ name }}</p>
<button @click="update">Update</button>
```

#### Reactive can lose its reactivity

Reactive can lose its reactivity if you try to pass one of its properties instead of the whole object.
```js
const obj = reactive({ name: 'Luigi' })
return { name: obj.name } // Reactivity of 'name' is lost, since 'name' becomes just a plain value
```
```html
<p>{{ name }}</p>
<button @click="name = 'Yoshi'">Update</button> // Doesn't update on templates
```

* Correct way for reactive - "Always use the object itself, instead of primitive values or individual properties":
```js
const obj = reactive({ name: 'Luigi' })
const update = () => { obj.name = 'Yoshi 2' } // Updates on templates
return { obj, update }
```
```html
<p>{{ obj.name }}</p>
<button @click="obj.name = 'Yoshi'">Update</button> // Works
<button @click="update">Update 2</button>
```

* With ref:
```js
const obj = ref({ name: 'Luigi' })
const update = () => { obj.value.name = 'Yoshi 2' } // Updates on templates
return { obj, update }
```
```html
<p>{{ obj.name }}</p> <!-- Vue unwraps .value in templates; no need to use obj.name.value -->
<button @click="obj.name = 'Yoshi'">Update</button> // Works
<button @click="update">Update 2</button>
```

#### Resume

If you use `reactive`, always work with the entire object, both when creating and when returning it from a composition function.<br>
Avoid using `reactive` for primitive values or individual properties in the return.<br>
For both reasons, `ref` is generally safer and simpler, especially when dealing with primitive values or when exposing properties individually from composition functions.
