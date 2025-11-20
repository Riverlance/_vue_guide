## ▶ Reactive values — computed()

### What is it?

The `computed()` function lets you create reactive values that are **derived from other reactive data**.<br>
It automatically updates whenever the values it depends on change.<br>
Think of it as a *smart variable* that recalculates itself only when needed.

> [!tip]
> This content is related to `ref()`/`reactive()` and `watch()`.

### When to use it?

Use `computed()` when you need to **transform, filter or combine existing reactive data** and always want the result to stay updated automatically.

Examples:
- Filtering a list
- Formatting text
- Creating dynamic values based on state

### How to use it?

* Basic usage

```html
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('Mario')
const lastName = ref('Bros')

const fullName = computed(() => `${firstName.value} ${lastName.value}`)
</script>

<template>
  <p>{{ fullName }}</p> <!-- Output: Mario Bros -->
</template>
```

* Example: filtering a list (Composition API)

```html
<template>
  <div>
    <input type="text" v-model="search">
    <p>Search term: {{ search }}</p>

    <div v-for="name in matchingNames" :key="name">
      {{ name }}
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const search = ref('')
const names = ref(['Mario', 'Yoshi', 'Luigi', 'Toad', 'Bowser', 'Koopa', 'Peach'])

const matchingNames = computed(() => {
  return names.value.filter((name) =>
    name.toLowerCase().includes(search.value.toLowerCase())
  )
})
</script>
```

* Old way (Options API)

```html
<script>
export default {
  data() {
    return {
      search: '',
      names: ['Mario', 'Yoshi', 'Luigi', 'Toad', 'Bowser', 'Koopa', 'Peach']
    }
  },
  computed: {
    matchingNames() {
      return this.names.filter((name) =>
        name.toLowerCase().includes(this.search.toLowerCase())
      )
    }
  }
}
</script>
```

### Extra: Centralize state logic with a reactive getter (`computed()`)

#### Problem — Why a plain variable gets messy

When many parts of your code can start or stop a game, a single `playing = true` / `playing = false` variable becomes hard to trust. Different places may set it incorrectly or forget to update it. Over time you don't know which code path actually determines whether the game is running.

Wrong approach (multiple functions using `playing` variable):

```html
<template>
  <div>
    <!-- Print the result -->
    <p>Playing: {{ playing ? 'Yes' : 'No' }}</p>

    <button @click="startGame">Start</button>
    <button @click="onPlayerDisconnected">Simulate Disconnect</button>
    <button @click="onLevelLoaded">Simulate Level Load</button>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const playing = ref(false)

// 1) User clicks "Start"
function startGame() {
  playing.value = true
}

// 2) Level loader finishes (delayed) and also sets playing
function onLevelLoaded() {
  loadLevel(() => {
    playing.value = true // Tries to start the game after load
  })
}

// 3) Network / player events also toggle it
function onPlayerDisconnected() {
  playing.value = false // Stop when player disconnects
}

// Simulated async loader with callback
function loadLevel(cb) {
  setTimeout(cb, 500)
}

console.log(playing.value) // boolean
</script>
```

Why this is problematic:

* **Race conditions**: `onLevelLoaded()` finishes later and may set `playing = true` after `onPlayerDisconnected()` has already set it to `false`, leaving the app in the wrong state.
* **Order-dependent bugs**: The final state depends only on who wrote it last, not on the actual game rules.
* **Hard to maintain**: As the code grows, multiple functions try to control playing, making it impossible to guarantee when the game is actually "playing".
* **Missing checks**: Handlers may set `playing = true` without validating preconditions (player connected, resources loaded, etc), generating invalid states.

#### Solution — Rules in one reactive function

Make a `computed` property (a reactive getter) that contains all the logic to decide whether the game is playing. Instead of writing to a boolean in many places, update only the source data (like timers, player existence, flags, etc) and ask the `computed` property whether the game is playing.

Centralize the decision logic in a `computed` property (getter). Update only the simple source values (timers, player existence, flags). Then ask the `computed` property whether the game is playing — this makes your code clearer, safer, and easier to maintain.

Correct approach (using `computed` for logic):

```html
<template>
  <div>
    <!-- Print the result -->
    <p>Playing: {{ isPlaying ? 'Yes' : 'No' }}</p>

    <button @click="startGame">Start Game</button>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// Reactive data (as you wish)
const playingTime = ref(0)

// Change the reactive data (as you wish)
function startGame() {
  playingTime.value = 1
}

// This reactive function implements all the logics to check if the game is playing, instead of using a simple variable `playing`
const isPlaying = computed(() => {
  if (playingTime.value < 1)
    return false

  // Other logics in here (as you wish)

  return true
})

// Print the result
console.log(isPlaying.value) // boolean
</script>
```

Why this is better:

Put the rules in one place (a `computed` that reads the real sources: load status, player connected, timers). Then handlers only update the source data (e.g., `loaded = true`, `player = {...}`, `startTime = Date.now()`), and you ask the `computed` `isPlaying` for the state result. This prevents races/order bugs (as mentioned) and centralizes the logic, so it's easy to understand and test.

#### Advantages

* **Reactive & efficient**: `computed` updates automatically when its inputs change and is cached (it recalculates only when needed).
* **Single source of truth**: All rules live in one place, so it's easier to understand and change.
* **Less bugs**: You avoid many parts of code fighting over the same boolean.
* **Easier testing**: You can test the `computed` logic alone without simulating many side effects.
* **Clear intent**: Reading `isPlaying` shows intent — it's a derived state, not a variable to toggle everywhere.

#### Other places this pattern helps

In short, any variable that is updated from many places, making it hard to track or understand. But there are some real-world examples below:

* **Buttons state** (`create` / `edit` / `delete`): Instead of toggling variables like `canCreate`, `canEdit` or `canDelete` from many handlers, compute each from item selection, permissions, and form validity.
* **Visibility of UI panels**: Compute whether a panel should show based on user role, feature flags and current route — not by setting `showPanel = true` in many handlers.
* **Form submit enabled/disabled**: Use a `computed` that checks all field validity rules.
* **Game or app modes**: Decide whether you're in "edit mode", "preview mode" or "play mode" by computing from several flags rather than flipping a mode variable everywhere.
