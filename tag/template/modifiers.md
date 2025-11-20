## ▶ Event Modifiers

### What is it?

When you add an event listener like `@click`, it works just like a regular DOM event.<br>
However, sometimes you need to adjust how or when the event runs.<br>
For example, running it only once.<br>
<br>
There are special modifiers that you can add to event listeners to change this behavior easily.<br>
Modifiers are added after the event name, like `@click.prevent`, `@submit.stop`, `@click.self` and so on.

> [!tip]
> Event modifiers are shortcuts for common `event` behaviors (like `preventDefault`, `stopPropagation`, etc).<br>
> Readmore: https://vuejs.org/guide/essentials/event-handling

> [!tip]
> `@click` is an alias for `v-on:click`.

### When to use it?

Use event modifiers when you need more control over how an event behaves.<br>
For example:<br>
* You want to prevent a form from submitting the normal way.
* You want to stop a click inside a modal from closing it.
* You want a button to only respond the first time it’s clicked.

### How to use it?

* `.stop` — Stop event from bubbling up
> This stops the event from reaching parent elements.
```html
<!-- Clicking inside modal won't close it -->
<div class="backdrop" @click="closeModal">
  <div class="modal" @click.stop>
    This click will NOT close the modal.
  </div>
</div>
```

* `.prevent` — Prevent default behavior
> This stops the browser's default action.
```html
<!-- Prevent the form from actually submitting and execute handleSubmit instead -->
<form @submit.prevent="handleSubmit">
  <button type="submit">Send</button>
</form>
```

* `.self` — Only trigger if clicked on the exact element
> Use this when you only want the event to fire if the element itself is clicked, not its children.
```html
<!-- Close modal only if clicking on backdrop (shadow around), not on modal itself -->
<div class="backdrop" @click.self="closeModal">
  <div class="modal">
    Clicking here won't close the modal
  </div>
</div>
```

* `.capture` — Trigger during the capture phase
> With `.capture`, `outerClicked` runs before `innerClicked`: Outer --> Inner.<br>
> Without `.capture` (default behavior), `innerClicked` runs first due to event bubbling: Inner --> Outer.
> Useful when you want parent elements to intercept clicks before children.
```html
<div @click.capture="outerClicked">
  <button @click="innerClicked">Click me</button>
</div>
```

* `.once` — Trigger the event only once
> Runs the event only on the first interaction.
```html
<button @click.once="handleClick">Click me only once</button>
```

* `.left`, `.right`, `.middle` — Specific mouse button
> Triggers the event only if the specified mouse button is used.
```html
<button @click.left="handleClick">Left Click Only</button>
<button @click.right="handleClick">Right Click Only</button>
<button @click.middle="handleClick">Middle Click Only</button>
<button @click.ctrl="handleClick">Ctrl + Click to Copy</button> <!-- Left Click + Ctrl -->
```

* `.ctrl`, `.shift`, `.alt`, .`meta` (⌘ Mac, ⊞ Windows) — Trigger event with key/button
> Trigger events only when a specific key or button is pressed.
```html
<button @click.ctrl="handleClick">Ctrl + Click</button> <!-- Requires Ctrl to be held, but not it only -->
<button @click.shift="handleClick">Shift + Click</button> <!-- Requires Shift to be held, but not it only -->
<button @click.alt="handleClick">Alt + Click</button> <!-- Requires Alt to be held, but not it only -->
<button @click.meta="handleClick">Meta + Click</button> <!-- Requires Meta to be held, but not it only -->

<button @click.ctrl.exact="handleClick">Ctrl + Click</button> <!-- Requires Ctrl only to be held -->
<button @click.shift.exact="handleClick">Shift + Click</button> <!-- Requires Shift only to be held -->
<button @click.alt.exact="handleClick">Alt + Click</button> <!-- Requires Alt only to be held -->
<button @click.meta.exact="handleClick">Meta + Click</button> <!-- Requires Meta only to be held -->
```

* `.keyup`, `.keydown` and others
> Useful for keyboard shortcuts and custom key commands.
```html
<input @keyup.enter.ctrl="submitForm" placeholder="Ctrl + Enter to submit" /> <!-- Ctrl + Enter -->
<input @keyup.enter.shift="addNewLine" placeholder="Shift + Enter to add line" /> <!-- Shift + Enter -->
<input @keyup.s.alt="search" placeholder="Alt + S to search" /> <!-- Alt + S -->
<!-- `.keypress` is deprecated -->

<!-- For more complex handling -->
<input @keydown="handleKeyDown" />
<script setup>
function handleKeyDown(event) {
  if (event.key === 'Backspace')
    ; // Handle backspace
}
</script>
```

> [!tip]
> After `click`, `keyup`, `keydown` and so on, the order does not matter.<br>
> For example: `@click.ctrl.shift.exact`, `@click.shift.ctrl.exact`, `@click.exact.shift.ctrl`.<br>
> They are all correct.
