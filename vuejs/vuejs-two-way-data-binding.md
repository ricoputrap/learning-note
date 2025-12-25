# Vue.js Two-Way Data Binding

## 1. Basic Text Input

```vue
<script setup>
import { ref } from "vue";

const text = ref("");
</script>

<input v-model="text" />
```

## 2. Input Modifiers (lazy, number, trim)

```vue
<script setup lang="ts">
import { ref } from "vue";

const lazyText = ref("");
const numberValue = ref(0);
const trimmedText = ref("");
</script>

<template>
  <div>
    <!-- .lazy: Syncs after "change" event instead of "input" -->
    <label>Lazy (updates on blur):</label>
    <input v-model.lazy="lazyText" />
    <p>{{ lazyText }}</p>

    <!-- .number: Automatically typecasts to number -->
    <label>Number (auto typecast):</label>
    <input v-model.number="numberValue" type="number" />
    <p>Value: {{ numberValue }} (Type: {{ typeof numberValue }})</p>

    <!-- .trim: Automatically trims whitespace -->
    <label>Trim (removes whitespace):</label>
    <input v-model.trim="trimmedText" />
    <p>"{{ trimmedText }}"</p>
  </div>
</template>
```

## 3. Textarea

```vue
<script setup lang="ts">
import { ref } from "vue";

const message = ref("");
</script>

<template>
  <div>
    <textarea
      v-model="message"
      placeholder="Enter a multi-line message"
      rows="4"
    ></textarea>
    <p style="white-space: pre-line">{{ message }}</p>
  </div>
</template>
```

## 4. Checkbox

```vue
<script setup lang="ts">
import { ref } from "vue";

// single checkbox
const checked = ref(false);

// Multiple checkboxes bound to an array
const checkedNames = ref<string[]>([]);
</script>

<template>
  <div>
    <!-- Single checkbox -->
    <label>
      <input type="checkbox" v-model="checked" />
      Subscribe to newsletter
    </label>
    <p>Checked: {{ checked }}</p>

    <!-- Multiple checkboxes -->
    <div>
      <label>
        <input type="checkbox" value="Vue" v-model="checkedNames" />
        Vue
      </label>
      <label>
        <input type="checkbox" value="React" v-model="checkedNames" />
        React
      </label>
      <label>
        <input type="checkbox" value="Angular" v-model="checkedNames" />
        Angular
      </label>
      <label>
        <input type="checkbox" value="Svelte" v-model="checkedNames" />
        Svelte
      </label>
    </div>
    <p>Selected frameworks: {{ checkedNames }}</p>
  </div>
</template>
```

## 5. Radio Buttons

```vue
<script setup lang="ts">
import { ref } from "vue";

const picked = ref("");
</script>

<template>
  <div>
    <label>
      <input type="radio" value="Vue" v-model="picked" />
      Vue
    </label>
    <label>
      <input type="radio" value="React" v-model="picked" />
      React
    </label>
    <label>
      <input type="radio" value="Angular" v-model="picked" />
      Angular
    </label>
    <p>Selected: {{ picked }}</p>
  </div>
</template>
```

## 6. Select Dropdown

```vue
<script setup lang="ts">
import { ref } from "vue";

// single selection
const selected = ref("");

// multiple selection
const multiSelected = ref<string[]>([]);

// dynamic options
const options = ref([
  { text: "Vue.js", value: "vue" },
  { text: "React", value: "react" },
  { text: "Angular", value: "angular" },
  { text: "Svelte", value: "svelte" },
]);
</script>

<template>
  <div>
    <!-- Single select -->
    <select v-model="selected">
      <option disabled value="">Please select one</option>
      <option>Vue</option>
      <option>React</option>
      <option>Angular</option>
    </select>
    <p>Selected: {{ selected }}</p>

    <!-- Multiple select -->
    <select v-model="multiSelected" multiple>
      <option>Vue</option>
      <option>React</option>
      <option>Angular</option>
      <option>Svelte</option>
    </select>
    <p>Selected: {{ multiSelected }}</p>

    <!-- Dynamic options with v-for -->
    <select v-model="selected">
      <option
        v-for="option in options"
        :key="option.value"
        :value="option.value"
      >
        {{ option.text }}
      </option>
    </select>
  </div>
</template>
```

## 7. Custom Components with `v-model`

```vue
<!-- CustomInput.vue -->
<script setup lang="ts">
// The component receives modelValue as a prop
defineProps<{
  modelValue: string;
}>();

// And emits 'update:modelValue' to update the parent
const emit = defineEmits<{
  "update:modelValue": [value: string];
}>();

function updateValue(event: Event) {
  const target = event.target as HTMLInputElement;
  emit("update:modelValue", target.value);
}
</script>

<template>
  <input :value="modelValue" @input="updateValue" />
</template>
```

```vue
<!-- Parent Component -->
<script setup lang="ts">
import { ref } from "vue";
import CustomInput from "./CustomInput.vue";

const text = ref("");
</script>

<template>
  <CustomInput v-model="text" />
  <p>Text: {{ text }}</p>
</template>
```

## 8. Multiple `v-model` Bindings

```vue
<!-- UserForm.vue -->
<script setup lang="ts">
defineProps<{
  firstName: string;
  lastName: string;
}>();

const emit = defineEmits<{
  "update:firstName": [value: string];
  "update:lastName": [value: string];
}>();
</script>

<template>
  <div>
    <input
      :value="firstName"
      @input="
        emit('update:firstName', ($event.target as HTMLInputElement).value)
      "
      placeholder="First Name"
    />
    <input
      :value="lastName"
      @input="
        emit('update:lastName', ($event.target as HTMLInputElement).value)
      "
      placeholder="Last Name"
    />
  </div>
</template>
```

```vue
<!-- Parent Component -->
<script setup lang="ts">
import { ref } from "vue";
import UserForm from "./UserForm.vue";

const firstName = ref("");
const lastName = ref("");
</script>

<template>
  <UserForm v-model:first-name="firstName" v-model:last-name="lastName" />
  <p>Full Name: {{ firstName }} {{ lastName }}</p>
</template>
```
