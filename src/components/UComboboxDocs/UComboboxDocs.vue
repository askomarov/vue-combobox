<script setup lang="ts">
import { ref } from 'vue'
import UCombobox from '../UCombobox/UCombobox.vue'

interface Item {
  id: string
  name: string
}

// Sample data for documentation
const simpleItems = ref<Item[]>([
  { id: '1', name: 'Apple' },
  { id: '2', name: 'Banana' },
  { id: '3', name: 'Orange' },
  { id: '4', name: 'Mango' },
  { id: '5', name: 'Pineapple' },
])

// Basic example
const basicSelected = ref<Item | null>(null)
const basicInput = ref('')

// Error example
const errorSelected = ref<Item | null>(null)
const errorInput = ref('')
const errorExample = ref(true)

// Loading example
const loadingSelected = ref<Item | null>(null)
const loadingInput = ref('')
const loadingExample = ref(true)

interface User {
  id: number
  firstName: string
  lastName: string
  email: string
  image?: string
}
// Custom display example
const users = ref<User[]>([
  {
    id: 1,
    firstName: 'John',
    lastName: 'Doe',
    email: 'john@example.com',
    image: 'https://randomuser.me/api/portraits/men/1.jpg',
  },
  {
    id: 2,
    firstName: 'Jane',
    lastName: 'Smith',
    email: 'jane@example.com',
    image: 'https://randomuser.me/api/portraits/women/2.jpg',
  },
  {
    id: 3,
    firstName: 'Alex',
    lastName: 'Johnson',
    email: 'alex@example.com',
    image: 'https://randomuser.me/api/portraits/men/3.jpg',
  },
])

const selectedUser = ref<User | null>(null)
const userInput = ref('')

const displayUserValue = (user: User | null) => {
  return user ? `${user.firstName} ${user.lastName}` : ''
}

// Function to display the selected fruit
const displayFruitValue = (item: Item | null) => {
  return item ? `${item.name}` : ''
}

// Filtering example
const filterItems = ref<Item[]>([
  { id: '1', name: 'Apple' },
  { id: '2', name: 'Banana' },
  { id: '3', name: 'Orange' },
  { id: '4', name: 'Mango' },
  { id: '5', name: 'Pineapple' },
  { id: '6', name: 'Strawberry' },
  { id: '7', name: 'Blueberry' },
  { id: '8', name: 'Raspberry' },
])

const filteredSelected = ref<Item | null>(null)
const filteredInput = ref('')
const filteringEnabled = ref(true)

// Custom filtering function example
const customFilterSelected = ref<Item | null>(null)
const customFilterInput = ref('')

// Custom filtering function
const customFilterFunction = (item: Item, query: string) => {
  if (!query) return true
  return item.name.toLowerCase().startsWith(query.toLowerCase())
}
</script>

<template>
  <div class="u-combobox-docs">
    <h1 class="text-2xl font-bold mb-6">UCombobox - Documentation</h1>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">Description</h2>
      <p class="mb-4">
        UCombobox is a versatile component for selecting from a list of options with search support.
        The component combines the functionality of a dropdown list and an input field, allowing
        users to quickly find and select the desired items.
      </p>
      <p class="mb-4">Key features:</p>
      <ul class="list-disc pl-6 mb-4">
        <li>Support for searching through the list of options</li>
        <li>Keyboard navigation support (up/down arrows, Enter, Escape)</li>
        <li>Custom display of options through slots</li>
        <li>Support for asynchronous data loading</li>
        <li>Handling of loading and error states</li>
        <li>Ability to clear the selected value</li>
        <li>Customizable display of the selected value</li>
        <li>Support for two-way binding for selected value and input text</li>
      </ul>
    </section>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">Usage Examples</h2>

      <!-- Basic example -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Basic Example</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="basicSelected"
            v-model:input="basicInput"
            :options="simpleItems"
            :display-value="displayFruitValue"
            placeholder="Select a fruit"
            show-toggle-button
          />
        </div>
        <div v-if="basicSelected" class="mb-2 text-sm">
          Selected: {{ basicSelected.name }} (ID: {{ basicSelected.id }})
        </div>
        <div class="mb-2 text-sm">Current input value: {{ basicInput }}</div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
&lt;script setup&gt;
import { ref } from 'vue'
import UCombobox from './components/UCombobox/UCombobox.vue'

interface Item {
  id: number
  name: string
}

const items = ref&lt;Item[]&gt;([
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Orange' },
])

const selected = ref&lt;Item | null&gt;(null)
const inputValue = ref('')

// Function to display the selected fruit
const displayFruitValue = (item: Item | null) => {
  return item ? item.name : ''
}
&lt;/script&gt;

&lt;template&gt;
  &lt;UCombobox
    v-model="selected"
    v-model:input="inputValue"
    :options="items"
    :display-value="displayFruitValue"
    placeholder="Select a fruit"
    show-toggle-button
  /&gt;
&lt;/template&gt;</pre
        >
      </div>

      <!-- Custom display example -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Custom Display of Options Through Slots</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="selectedUser"
            v-model:input="userInput"
            :options="users"
            :display-value="displayUserValue"
            placeholder="Select a user"
          >
            <template #option-content="{ option }">
              <div class="flex items-center gap-2">
                <img
                  v-if="option && option.image"
                  :src="option.image"
                  :alt="option.lastName || ''"
                  class="w-8 h-8 rounded-full"
                />
                <div>
                  <div class="font-medium">
                    {{ option && option.firstName ? option.firstName : '' }}
                    {{ option && option.lastName ? option.lastName : '' }}
                  </div>
                  <div v-if="option && option.email" class="text-xs text-gray-500">
                    {{ option.email }}
                  </div>
                </div>
              </div>
            </template>
          </UCombobox>
        </div>
        <div class="mb-2 text-sm">Current input value: {{ userInput }}</div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
&lt;UCombobox
  v-model="selectedUser"
  v-model:input="userInput"
  :options="users"
  :display-value="displayUserValue"
  placeholder="Select a user"
&gt;
  &lt;template #option-content="{ option }"&gt;
    &lt;div class="flex items-center gap-2"&gt;
      &lt;img
        v-if="option && option.image"
        :src="option.image"
        :alt="option.lastName || ''"
        class="w-8 h-8 rounded-full"
      /&gt;
      &lt;div&gt;
        &lt;div class="font-medium"&gt;
          &#123;&#123; option && option.firstName ? option.firstName : '' &#125;&#125;
          &#123;&#123; option && option.lastName ? option.lastName : '' &#125;&#125;
        &lt;/div&gt;
        &lt;div v-if="option && option.email" class="text-xs text-gray-500"&gt;
          &#123;&#123; option && option.email ? option.email : '' &#125;&#125;
        &lt;/div&gt;
      &lt;/div&gt;
    &lt;/div&gt;
  &lt;/template&gt;
&lt;/UCombobox&gt;</pre
        >
      </div>

      <!-- Loading state example -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Handling Loading State</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="loadingSelected"
            v-model:input="loadingInput"
            :display-value="(item) => (item ? item.name : '')"
            :is-loading="loadingExample"
            :options="[]"
            placeholder="Loading data..."
          />
        </div>
        <button
          @click="loadingExample = !loadingExample"
          class="px-3 py-1 bg-accent-pink text-white rounded-md"
        >
          {{ loadingExample ? 'Stop loading' : 'Show loading' }}
        </button>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto mt-3">
&lt;UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :display-value="(item) => (item ? item.name : '')"
  :options="[]"
  :is-loading="isLoading"
  placeholder="Loading data..."
/&gt;</pre
        >
      </div>

      <!-- Error example -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Error Handling</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="errorSelected"
            v-model:input="errorInput"
            :display-value="(item) => (item ? item.name : '')"
            :invalid="errorExample"
            :options="[]"
            :error="errorExample ? 'Failed to load data' : null"
            placeholder="Example with error"
          />
        </div>
        <button
          @click="errorExample = !errorExample"
          class="px-3 py-1 bg-accent-pink text-white rounded-md"
        >
          {{ errorExample ? 'Hide error' : 'Show error' }}
        </button>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto mt-3">
&lt;UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :display-value="(item) => (item ? item.name : '')"
  :options="[]"
  :invalid="true"
  :error="'Failed to load data'"
  placeholder="Example with error"
/&gt;</pre
        >
      </div>

      <!-- Filtering example -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Local Data Filtering</h3>
        <p class="mb-4 text-sm text-gray-600">
          The component can automatically filter local data as text is entered in the search field.
          This is useful when all data is available on the client and there's no need to send
          requests to the server.
        </p>

        <div class="flex items-center gap-2 mb-4">
          <label class="inline-flex items-center cursor-pointer">
            <input type="checkbox" v-model="filteringEnabled" class="sr-only peer" />
            <div
              class="relative w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-accent-pink"
            ></div>
            <span class="ml-3 text-sm font-medium">
              {{ filteringEnabled ? 'Filtering enabled' : 'Filtering disabled' }}
            </span>
          </label>
        </div>

        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="filteredSelected"
            v-model:input="filteredInput"
            :options="filterItems"
            :display-value="displayFruitValue"
            :enable-filtering="filteringEnabled"
            placeholder="Type to filter..."
            show-toggle-button
          />
        </div>
        <div class="mb-2 text-sm">Current input value: {{ filteredInput }}</div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
&lt;UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :options="items"
  :display-value="displayFruitValue"
  :enable-filtering="true"
  placeholder="Type to filter..."
  show-toggle-button
/&gt;</pre
        >

        <h4 class="font-medium mb-2 mt-4">Custom Filtering Function</h4>
        <p class="mb-4 text-sm text-gray-600">
          You can define your own filtering logic by passing a function to the filterFunction prop.
        </p>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="customFilterSelected"
            v-model:input="customFilterInput"
            :options="filterItems"
            :display-value="displayFruitValue"
            :enable-filtering="true"
            :filter-function="customFilterFunction"
            placeholder="Enter the beginning of a name..."
          />
        </div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
        // Filter function that matches the beginning of the string
        const customFilterFunction = (item, query) => {
          if (!query) return true
          return item.name.toLowerCase().startsWith(query.toLowerCase())
        }

        &lt;UCombobox
          v-model="selected"
          v-model:input="inputValue"
          :options="items"
          :display-value="displayFruitValue"
          :enable-filtering="true"
          :filter-function="customFilterFunction"
          placeholder="Enter the beginning of a name..."
        /&gt;</pre
        >
      </div>
    </section>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">API</h2>

      <h3 class="font-medium mb-2">Props</h3>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse mb-6">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Name</th>
              <th class="py-2 px-4 border text-left">Type</th>
              <th class="py-2 px-4 border text-left">Default</th>
              <th class="py-2 px-4 border text-left">Description</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">modelValue</td>
              <td class="py-2 px-4 border">T | null</td>
              <td class="py-2 px-4 border">null</td>
              <td class="py-2 px-4 border">Selected value (v-model)</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">input</td>
              <td class="py-2 px-4 border">string</td>
              <td class="py-2 px-4 border">''</td>
              <td class="py-2 px-4 border">Input field value (v-model:input)</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">options</td>
              <td class="py-2 px-4 border">Array&lt;T&gt;</td>
              <td class="py-2 px-4 border">[]</td>
              <td class="py-2 px-4 border">Array of options to choose from</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">displayValue</td>
              <td class="py-2 px-4 border">(item: T | null) => string</td>
              <td class="py-2 px-4 border">(item) => String(item)</td>
              <td class="py-2 px-4 border">
                Function to display the option value in the input field
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">isLoading</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Data loading indicator</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">error</td>
              <td class="py-2 px-4 border">string | null</td>
              <td class="py-2 px-4 border">null</td>
              <td class="py-2 px-4 border">Error message</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">placeholder</td>
              <td class="py-2 px-4 border">string</td>
              <td class="py-2 px-4 border">'Select an option...'</td>
              <td class="py-2 px-4 border">Placeholder for the input field</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">class</td>
              <td class="py-2 px-4 border">string</td>
              <td class="py-2 px-4 border">''</td>
              <td class="py-2 px-4 border">CSS classes for the root element</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">invalid</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Invalid state flag</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">showToggleButton</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Show button to open/close the list</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">enableFiltering</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Enables/disables automatic filtering of local data</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">filterFunction</td>
              <td class="py-2 px-4 border">(option: T, query: string) => boolean</td>
              <td class="py-2 px-4 border">undefined</td>
              <td class="py-2 px-4 border">Custom function for filtering options</td>
            </tr>
          </tbody>
        </table>
      </div>

      <h3 class="font-medium mb-2">Events</h3>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse mb-6">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Name</th>
              <th class="py-2 px-4 border text-left">Arguments</th>
              <th class="py-2 px-4 border text-left">Description</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">update:modelValue</td>
              <td class="py-2 px-4 border">(value: T | null)</td>
              <td class="py-2 px-4 border">Triggered when the selected value changes</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">update:input</td>
              <td class="py-2 px-4 border">(value: string)</td>
              <td class="py-2 px-4 border">Triggered when the text in the input field changes</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">change</td>
              <td class="py-2 px-4 border">(value: T | null)</td>
              <td class="py-2 px-4 border">Triggered when an option is selected</td>
            </tr>
          </tbody>
        </table>
      </div>

      <h3 class="font-medium mb-2">Slots</h3>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Name</th>
              <th class="py-2 px-4 border text-left">Slot Props</th>
              <th class="py-2 px-4 border text-left">Description</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">icon</td>
              <td class="py-2 px-4 border">-</td>
              <td class="py-2 px-4 border">Icon in the input field</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">option-content</td>
              <td class="py-2 px-4 border">{ option }</td>
              <td class="py-2 px-4 border">Content of each option in the dropdown list</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">empty</td>
              <td class="py-2 px-4 border">-</td>
              <td class="py-2 px-4 border">Content displayed when the list of options is empty</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">Keyboard Navigation</h2>
      <p class="mb-4">The component supports the following key combinations:</p>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Key</th>
              <th class="py-2 px-4 border text-left">Action</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">Down arrow (↓)</td>
              <td class="py-2 px-4 border">
                Opens the dropdown list if it is closed. Moves the focus to the next item in the
                list.
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">Up arrow (↑)</td>
              <td class="py-2 px-4 border">
                Opens the dropdown list if it is closed. Moves the focus to the previous item in the
                list.
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">Enter</td>
              <td class="py-2 px-4 border">
                Selects the item currently in focus and closes the dropdown list.
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">Escape</td>
              <td class="py-2 px-4 border">Closes the dropdown list.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>
  </div>
</template>

<style scoped>
.u-combobox-docs {
  max-width: 1000px;
  margin: 0 auto;
}
</style>
