## ▶ Sending events out — defineEmits()

### What is that?

The `defineEmits()` function allows a child component to send events to its parent.<br>
It is a special function that works only inside `<script setup>` and is used to define and trigger custom events.<br>
Events are how a child component communicates back to its parent, usually to notify it that something happened (like a click or form submission).

> [!tip]
> This content is related to `<script setup>` and `defineProps()`.

### When to use it?

Use `defineEmits()` when you want the component to send actions or updates to the parent.

### How to use it?

* Basic usage (modern way)

```html
<!-- ChildComponent.vue -->

<template>
  <button @click="send">Send</button>
</template>

<script setup>
  const emit = defineEmits()
  function send() {
    emit('custom-event', 'Hello!')
  }
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent @custom-event="handleCustomEvent" />
</template>

<script setup>
  function handleCustomEvent(payload) {
    console.log(payload) // Output: Hello!
  }
</script>
```

* Basic usage (old way)

```html
<!-- ChildComponent.vue -->

<template>
  <button @click="send">Send</button>
</template>

<script>
export default {
  emits: ['custom-event'],
  methods: {
    send() {
      this.$emit('custom-event', 'Hello!')
    }
  }
}
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent @custom-event="handleCustomEvent" />
</template>

<script>
import ChildComponent from './ChildComponent.vue'

export default {
  components: { ChildComponent },
  methods: {
    handleCustomEvent(payload) {
      console.log(payload) // Output: Hello!
    }
  }
}
</script>
```

* With defined structure

```html
<!-- ChildComponent.vue -->

<template>
  <button @click="send">Send</button>
</template>

<script setup>
  const emit = defineEmits(['custom-event', 'submit'])

  function send() {
    emit('custom-event', 'Hello!')
    emit('submit', 1)
  }
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent @custom-event="handleCustomEvent" @submit="handleSubmit" />
</template>

<script setup>
  function handleCustomEvent(payload) {
    console.log(payload) // Output: Hello!
  }

  function handleSubmit(id) {
    console.log(id) // Output: 1
  }
</script>
```

* With payload validation (optional)

```html
<!-- ChildComponent.vue -->

<template>
  <button @click="send">Send</button>
</template>

<script setup>
  const emit = defineEmits({
    'custom-event': (msg) => typeof msg === 'string',
    'submit': (id) => typeof id === 'number'
  })

  function send() {
    emit('custom-event', 'Hello!')
    emit('submit', 1)
  }
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent @custom-event="handleCustomEvent" @submit="handleSubmit" />
</template>

<script setup>
  function handleCustomEvent(payload) {
    console.log(payload) // Output: Hello!
  }

  function handleSubmit(id) {
    console.log(id) // Output: 1
  }
</script>
```
