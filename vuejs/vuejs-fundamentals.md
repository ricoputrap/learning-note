# Vue.js Fundamentals

## 1. Interactivity

```vue
<script setup lang="ts">
import { ref, computed } from "vue";

// simple primitive value
const count = ref(0);

// derived value
const doubleCount = computed(() => count.value * 2);

function increment() {
  count.value++;
}

function decrement() {
  count.value--;
}

function reset() {
  count.value = 0;
}
</script>

<template>
  <div>
    <p>Count: {{ count }}</p>
    <p>Double Count: {{ doubleCount }}</p>
    <button @click="increment">Increment</button>
    <button @click="decrement">Decrement</button>
    <button @click="reset">Reset</button>
  </div>
</template>

<style scoped></style>
```

## 2. Data Fetching

### 2.1. Simple Data Fetching

```vue
<script setup lang="ts">
import { ref, onMounted } from "vue";

const user = ref(null);

onMounted(async () => {
  const res = await fetch("/api/user");
  user.value = await res.json();
});
</script>

<template>
  <div v-if="user">{{ user.name }}</div>
  <p v-else>Loading...</p>
</template>
```

### 2.2. Advanced Data Fetching

```vue
<script setup lang="ts">
import { ref, onMounted } from "vue";

interface User {
  name: string;
  email?: string;
}

const user = ref<User | null>(null);
const loading = ref(true);
const error = ref<string | null>(null);

onMounted(async () => {
  try {
    const res = await fetch("/api/user");
    if (!res.ok) throw new Error("Failed to fetch user");
    user.value = await res.json();
  } catch (e) {
    error.value = e instanceof Error ? e.message : "An error occurred";
  } finally {
    loading.value = false;
  }
});
</script>

<template>
  <div v-if="error">Error: {{ error }}</div>
  <div v-else-if="loading">Loading...</div>
  <div v-else-if="user">{{ user.name }}</div>
</template>
```

## 3. Two-Way Binding

```vue
<script setup>
import { ref } from "vue";

const text = ref("");
</script>

<input v-model="text" />
```

## 4. Forms

```vue
<script setup lang="ts">
import { ref } from "vue";

const email = ref("");
const password = ref("");

function submit() {
  console.log("Email:", email.value);
  console.log("Password:", password.value);
}
</script>

<template>
  <form @submit.prevent="submit">
    <label for="email">Email:</label>
    <input type="email" id="email" v-model="email" required />
    <label for="password">Password:</label>
    <input type="password" id="password" v-model="password" required />
    <button type="submit">Submit</button>
  </form>
</template>
```

## 5. Global State Management (Pinia)

### 5.1. Defining Stores with Options API

```typescript
export const useCounterStore = defineStore("counter", {
  state: () => ({ count: 0, name: "Eduardo", users: [] }),

  // getters are exactly the equivalent of
  // computed values for the state of a Store
  getters: {
    doubleCount: (state) => state.count * 2,

    // Getters are just computed properties behind the scenes,
    // so it's not possible to pass any parameters to them.
    // However, you can return a function from the getter to accept any arguments.
    getUserById: (state) => (id) => state.users.find((user) => user.id === id),

    // Note that when doing this, getters are not cached anymore.
    // They are simply functions you invoke.
    // You can, however, cache some results inside of the getter itself,
    // which is uncommon but should prove more performant.
    getActiveUserById(state) {
      const activeUsers = state.users.filter((user) => user.active);
      return (userId) => activeUsers.find((user) => user.id === userId);
    },
  },
  actions: {
    increment() {
      this.count++;
    },
  },
});
```

### 5.2. Defining Stores with Composition API

```typescript
import { defineStore } from "pinia";

export const useCounterStore = defineStore("counter", () => {
  // become state
  const count = ref(0);
  const name = ref("Eduardo");

  // become getters
  const doubleCount = computed(() => count.value * 2);

  // become actions
  function increment() {
    count.value++;
  }

  return { count, name, doubleCount, increment };
});
```

### 5.3. Using Stores in Components

```vue
<script setup>
import { useCounterStore } from "@/stores/counter";

const counter = useCounterStore();
</script>

<template>
  <div>
    <button @click="counter.increment">Increment</button>
    <input type="text" v-model="counter.name" />
    <p>Count: {{ counter.count }}</p>
    <p>Double Count: {{ counter.doubleCount }}</p>
  </div>
</template>
```
