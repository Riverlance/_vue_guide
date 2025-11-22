## ▶ Passing data in — defineProps()

### What is it?

The `defineProps()` function allows a child component to receive data from its parent.<br>
It is a special function that works only inside `<script setup>` and gives access to the component’s props.<br>
Props are how you pass values from a parent to a child component (like a user name, an ID or a configuration value).

> [!important]
> When you need to pass **complex content** (template code) to a component, it's usually better to use `slots` instead of `props`.<br>
> The `props` are best for simple values (such as text, numbers, booleans), while `slots` give more flexibility for structured content.

> [!tip]
> This content is related to `<script setup>`, `defineEmits()` and `withDefaults()`.

### When to use it?

Use `defineProps()` when you want the component to accept data from its parent.

### How to use it?

* Basic usage (modern way)

```html
<!-- ChildComponent.vue -->

<template>
  <p>{{ props.title }}</p> <!-- Output: Hello! -->
</template>

<script setup>
  const props = defineProps()
  console.log(props.title)
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent title="Hello!" />
</template>
```

* `defineProps()` with 2 variables

```html
<!-- ChildComponent.vue -->

<template>
  <p>{{ props.title }}</p> <!-- Output: Hello! -->
  <p>{{ props.count }}</p> <!-- Output: 7 -->
</template>

<script setup>
  const props = defineProps(['title', 'count'])
  console.log(props.title, props.count)
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent title="Hello!" count="7" />
</template>
```

* Basic usage (old way)

```html
<!-- ChildComponent.vue -->

<template>
  <p>{{ title }}</p> <!-- Output: Hello! -->
</template>

<script>
  export default {
    props: ['title']
  }
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent title="Hello!" />
</template>

<script>
  import ChildComponent from './ChildComponent.vue'

  export default {
    components: { ChildComponent }
  }
</script>
```

* With defined structure

```html
<!-- ChildComponent.vue -->

<template>
  <p>{{ props.title }}</p> <!-- Output: Hello! -->
  <p>{{ props.count }}</p> <!-- Output: 7 -->
  <p>{{ props.isActive ? 'Active' : 'Deactive' }}</p> <!-- Output: Active -->
</template>

<script setup>
  const props = defineProps({
    title: String,
    count: Number,
    isActive: Boolean
  })
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent title="Hello!" :count="7" :isActive="true" />
</template>
```

* `defineProps()` + `withDefaults()`

```html
<!-- ChildComponent.vue -->

<template>
  <p>{{ props.title }}</p> <!-- Output: Default title -->
  <p>{{ props.theme }}</p> <!-- Output: dark -->
</template>

<script setup>
  const props = withDefaults(defineProps({
    title: String,
    theme: String
  }), {
    title: 'Default title',
    theme: 'light'
  })
</script>

<!-- ParentComponent.vue -->

<template>
  <ChildComponent theme="dark" />
</template>
```
