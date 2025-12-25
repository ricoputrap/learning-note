# Vue.js Forms

## 1. Simple Form with Text Inputs

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

## 2. Complex Form

```vue
<script setup lang="ts">
import { ref, reactive } from "vue";

interface FormData {
  username: string;
  email: string;
  age: number | null;
  gender: string;
  country: string;
  interests: string[];
  newsletter: boolean;
  bio: string;
}

const form = reactive<FormData>({
  username: "",
  email: "",
  age: null,
  gender: "",
  country: "",
  interests: [],
  newsletter: false,
  bio: "",
});

const countries = ["USA", "UK", "Canada", "Australia", "Other"];

function handleSubmit() {
  console.log("Form submitted:", form);
}
</script>

<template>
  <form @submit.prevent="handleSubmit">
    <div>
      <label>Username:</label>
      <input v-model.trim="form.username" required />
    </div>

    <div>
      <label>Email:</label>
      <input type="email" v-model="form.email" required />
    </div>

    <div>
      <label>Age:</label>
      <input type="number" v-model.number="form.age" />
    </div>

    <div>
      <label>Gender:</label>
      <label
        ><input type="radio" value="male" v-model="form.gender" /> Male</label
      >
      <label
        ><input type="radio" value="female" v-model="form.gender" />
        Female</label
      >
      <label
        ><input type="radio" value="other" v-model="form.gender" /> Other</label
      >
    </div>

    <div>
      <label>Country:</label>
      <select v-model="form.country">
        <option v-for="country in countries" :key="country" :value="country">
          {{ country }}
        </option>
      </select>
    </div>

    <div>
      <label>Interests:</label>
      <label
        ><input type="checkbox" value="coding" v-model="form.interests" />
        Coding</label
      >
      <label
        ><input type="checkbox" value="design" v-model="form.interests" />
        Design</label
      >
      <label
        ><input type="checkbox" value="music" v-model="form.interests" />
        Music</label
      >
    </div>

    <div>
      <label>Bio:</label>
      <textarea v-model.trim="form.bio" rows="4"></textarea>
    </div>

    <div>
      <label>
        <input type="checkbox" v-model="form.newsletter" />
        Subscribe to newsletter
      </label>
    </div>

    <button type="submit">Submit</button>
  </form>

  <pre>{{ form }}</pre>
</template>
```
