## ▶ Custom content — `v-slot`

### What is that?

`v-slot`, or simply `#`, is a way to pass custom content into a component from the outside.<br>
Some components, like `<Modal>`, `<Card>`, `<Dropdown>`, are designed to be reused with different inner content.<br>
Slots gives you control over what goes inside those components.<br>
You can also use slots to get data from the child component, and use it inside the parent's template.

### When to use it?

* You want to inject custom content into a child component.
* You want to make components more flexible and reusable.
* You want to get variables or methods from the child and use them in your markup.

### How to use it?

```html
<!-- Modal.vue -->

<template>
  <div class="backdrop" @click.self="clickBackdrop">
    <div class="modal">
      <!-- 'Main' slot -->
      <slot :submit="submit"> <!-- Here goes the custom content of 'Main' slot ("Hello from parent!" and the button "Close") -->
        <p v-pre>
          <strong>
            <i>
              Default content, if this slot is not set by the parent.<br>
              You could have used <slot /> instead to have no default content.
              <!-- v-pre will treat <slot /> as plain text -->
            </i>
          </strong>
        </p>
      </slot>

      <!-- 'Links' slot -->
      <slot name="links" :shuffle="shuffle"> <!-- Here goes the custom content of 'Links' slot ("Sign up now!" and "More info") -->
        <p>
          <strong>
            <i>
              Default content, if this slot is not set by the parent.<br>
              You could have used {{ '<slot />' }} instead to have no default content.
              <!-- Without v-pre, using a simple string variable instead -->
            </i>
          </strong>
        </p>
      </slot>
    </div>
  </div>
</template>

<script setup>
  const emit = defineEmits(['click-backdrop', 'backdrop-clicked', 'submit'])
  function clickBackdrop() {
    // ... Optional logic before closing
    emit('click-backdrop') // Tells the parent to execute click-backdrop inside the modal; Here, the parent "closes" the modal (in this case, we are using `closeModal()` for it)
    // ... Optional logic after closing
    emit('backdrop-clicked') // Tells the parent to execute backdrop-clicked inside the modal (not in use, in this case)
  }

  // This function can be called by the parent via the scoped slot (see `@click="submit"`),
  // but it also emits a 'submit' event back to the parent when triggered (see `@submit="submitModal"`)
  function submit() {
    console.log('Submitted from the Modal!')
    emit('submit')
  }

  function shuffle() { console.log('Shuffle!') }
</script>

<!-- ParentComponent.vue -->

<template>
  <Modal @click-backdrop="closeModal" @submit="submitModal" #="{ submit }" v-if="isModalVisible">
    <!-- Inside this scope, you can use submit() -->

    <!-- Content to overwrite inside the 'Links' slot in Modal -->
    <template #links="{ shuffle }"> <!-- or <template v-slot:links="{ shuffle }"> -->
      <!-- Inside this scope, you can use shuffle(), but not submit() -->

      <a href="#">Sign up now!</a>
      <a href="#">More info</a>
    </template>

    <!-- Content to overwrite inside the 'Main' slot in Modal -->
    <h2>Hello from parent!</h2>
    <button @click="submit">Submit</button> <!-- This calls the submit() function from Modal, which emits the 'submit' event (see `@submit="submitModal"`) -->
    <button @click="closeModal">Close</button>
  </Modal>

  <button @click="openModal">Open Modal</button>
</template>

<script setup>
  import Modal from './Modal.vue'
  import { ref } from 'vue'

  const isModalVisible = ref(false)

  function submitModal() { console.log("Submitted from the Parent!") }
  function closeModal() { isModalVisible = false }
  function openModal() { isModalVisible = true }
</script>
```
