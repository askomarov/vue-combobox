# [Vue UCombobox](https://askomarov.github.io/vue-combobox/)

Universal UCombobox component for Vue 3, implemented using TypeScript and TailwindCSS.

## Description

UCombobox is a multifunctional dropdown list with search capability. The component combines the functionality of a dropdown and an input field, allowing users to quickly find and select the desired items.

### Features

- Support for searching through the list of options
- Keyboard navigation (up/down arrows, Enter, Escape)
- Customization through slots
- Asynchronous data loading
- Handling of loading and error states
- Local data filtering
- Custom filtering functions
- Complete TypeScript typing

### Demo

A demonstration of the component is available when running the project. It includes:

- Basic usage example
- Custom display of options
- Asynchronous data loading from API
- Handling errors and loading state
- Local data filtering

## Installation and Usage

### Installing Dependencies

```sh
pnpm install
```

### Running the Project for Development

```sh
pnpm dev
```

### Building for Production

```sh
pnpm build
```

### Usage Example

```vue
<script setup>
import { ref } from 'vue'
import UCombobox from './components/UCombobox/UCombobox.vue'

const items = ref([
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Orange' },
])

const selected = ref(null)
const inputValue = ref('')

const displayValue = (item) => (item ? item.name : '')
</script>

<template>
  <UCombobox
    v-model="selected"
    v-model:input="inputValue"
    :options="items"
    :display-value="displayValue"
    placeholder="Select a fruit"
    show-toggle-button
  />
</template>
```

### Local Filtering

The component supports automatic filtering of local data:

```vue
<UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :options="items"
  :display-value="displayValue"
  :enable-filtering="true"
  placeholder="Type to search"
/>
```

## Documentation

Detailed documentation with examples is available in the application interface when running the project.

## Tech Stack

- Vue 3
- TypeScript
- Tailwind CSS
- Vite
