## ▶ $refs

### What is that?

The $refs is used to access DOM elements or component instances directly from JavaScript.<br>
Use $refs as a last option when props, emits and reactive bindings don't meet your needs.

> [!warning]
> Vue recommends using $refs only when necessary, because it breaks the reactive flow.<br>
> Meaning you bypass Vue's preferred way of managing state and UI updates.

### When to use it?

* **Case 1**: To manipulate a DOM element directly (e.g., focus an input, add/remove classes).
* **Case 2**: To call methods on a child component (e.g., open a modal, reset a form).
* **Case 3**: To read internal state of a child component that's not exposed via props or emits.

### How to use it?

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
