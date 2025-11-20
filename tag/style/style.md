## ▶ Style — `<style>`

### What is it?

The `<style>` tag is used to define CSS or preprocessed CSS for a component.<br>
It can hold styles directly in the component or import external stylesheets.

> [!note]
> In components, `<style>` can use attributes like `scoped`, `module`, `lang` (preprocessors, e.g., `lang="scss"`), `src` and dynamic CSS with `v-bind()` (e.g., `:root { color: v-bind(--color) }`).

> [!tip]
> **Best practice**:<br>
> Keep component-specific styles co-located, and shared/base styles in imported files.

### When to use it?

Use `<style>` when you want to:
* Apply styles directly to a component (co-located styles).
* Import external stylesheets into the component file.
* Take advantage of scoped styles or CSS Modules.
* Apply preprocessor features (Sass, Less, PostCSS, etc) for richer styling.

### How to use it?

#### ▶ Write CSS/SCSS inside the tag

```html
<template>
  <div class="card">Hello</div>
</template>

<style>
.card {
  padding: 1rem;
  background-color: #f5f5f5;
}
</style>
```

#### ▶ Import external CSS/SCSS

* Use `src` attribute to import external CSS file:

```html
<!-- MyComponent.vue -->

<template>
  <div class="card">Hello</div>
</template>

<style src="./my-component.css"></style>
```

```css
/* my-component.css */

.card {
  padding: 1rem;
  background-color: #f5f5f5;
}
```

* Use `@import` inside the `<style>` tag to import external CSS file:

```html
<!-- MyComponent.vue -->

<template>
  <div class="card">Hello</div>
</template>

<style>
@import "@/assets/base.css";
</style>
```

```css
/* assets/base.css */

.card {
  padding: 1rem;
  background-color: #d0d0d0;
}
```

* Use `import` inside the `<script>` tag to import external CSS file:

> [!tip]
> Importing a CSS file via `<script>`, applies the styles globally to all components.

```html
<!-- MyComponent.vue -->

<template>
  <div class="card">Hello</div>
</template>

<script>
import './styles/global.css'
</script>
```

```css
/* styles/global.css */

.card {
  padding: 1rem;
  background-color: #e0e0e0;
}
```
