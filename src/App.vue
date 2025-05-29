<script setup lang="ts">
import { ref, watch } from 'vue'
import { useDebounceFn } from '@vueuse/core'
import UCombobox from './components/UCombobox/UCombobox.vue'
import UComboboxDocs from './components/UComboboxDocs/UComboboxDocs.vue'
import AppHeader from './components/App/Header/AppHeader.vue'

interface Item {
  id: number
  name: string
}
interface User {
  id: number
  firstName: string
  lastName: string
  email: string
  username: string
  image: string
}

// Переключатель для отображения документации или примеров
const showDocs = ref(false)

// Расширим список для демонстрации фильтрации
const items = ref<Item[]>([
  { id: 1, name: 'Apple' },
  { id: 2, name: 'Banana' },
  { id: 3, name: 'Orange' },
  { id: 4, name: 'Strawberry' },
  { id: 5, name: 'Blueberry' },
  { id: 6, name: 'Blackberry' },
  { id: 7, name: 'Raspberry' },
  { id: 8, name: 'Mango' },
  { id: 9, name: 'Pineapple' },
  { id: 10, name: 'Kiwi' },
])

const datafromApi = ref<User[]>([])
const userQuery = ref('')
const selectedUser = ref<User | null>(null)

const loading = ref(false)
const error = ref<string | null>(null)
const selected = ref<Item | null>(null)
const inputValue = ref('')

const selectedFiltered = ref<Item | null>(null)
const inputValueFiltered = ref('')

const fetchUsers = useDebounceFn(async (query: string) => {
  if (!query.trim()) {
    datafromApi.value = []
    return
  }

  loading.value = true
  error.value = null

  try {
    const response = await fetch(
      `https://dummyjson.com/users/search?q=${encodeURIComponent(query)}`,
    )
    if (!response.ok) {
      throw new Error('Ошибка при загрузке данных')
    }

    const data = await response.json()
    datafromApi.value = data.users || []
  } catch (err) {
    console.error('Ошибка запроса:', err)
    error.value = err instanceof Error ? err.message : 'Произошла ошибка'
    datafromApi.value = []
  } finally {
    loading.value = false
  }
}, 300)

// Отслеживаем изменения в поле поиска пользователей
watch(
  () => userQuery.value,
  (newQuery) => {
    fetchUsers(newQuery)
  },
)

// Функция для отображения данных пользователя
const displayUserValue = (user: User | null) => {
  return user ? `${user.firstName} ${user.lastName}` : ''
}

// Пример кастомной функции фильтрации
const customFilterFunction = (item: Item, query: string) => {
  if (!query) return true
  return item.name.toLowerCase().startsWith(query.toLowerCase())
}
</script>

<template>
  <h1 class="sr-only">
    UCombobox — это многофункциональный выпадающий список с возможностью поиска. Компонент сочетает
    функциональность выпадающего списка (dropdown) и поля ввода (input), позволяя пользователям
    быстро находить и выбирать нужные элементы.
  </h1>
  <AppHeader />
  <div class="container">
    <div class="navigation mb-6">
      <button
        @click="showDocs = false"
        class="px-4 py-2"
        :class="!showDocs ? 'bg-accent-pink text-white rounded-md' : 'text-accent-pink'"
      >
        Примеры
      </button>
      <button
        @click="showDocs = true"
        class="px-4 py-2 mr-2"
        :class="showDocs ? 'bg-accent-pink text-white rounded-md' : 'text-accent-pink'"
      >
        Документация
      </button>
    </div>

    <UComboboxDocs v-if="showDocs" />

    <main v-else class="grid gap-2">
      <p>пример с данными с API</p>
      <p>userQuery: {{ userQuery }}</p>
      <UCombobox
        v-model="selectedUser"
        v-model:input="userQuery"
        :options="datafromApi"
        :display-value="displayUserValue"
        :isLoading="loading"
        placeholder="Введите имя пользователя"
        :error="error"
        class="w-full max-w-xs"
        @change="console.log('Selected User:', $event)"
      >
        <!-- Переопределение стандартного отображения опций -->
        <template #option-content="{ option }">
          <div class="flex items-center gap-2">
            <!-- Например, добавить аватар -->
            <img
              v-if="option && option.image"
              :src="option.image"
              :alt="option.lastName || ''"
              class="w-6 h-6 rounded-full"
              width="24"
              height="24"
            />

            <!-- Основная информация -->
            <div>
              <div class="font-medium">
                {{ option.firstName }}
              </div>
              <div v-if="option && option.email" class="text-xs text-gray-500">
                {{ option.email }}
              </div>
            </div>
          </div>
        </template>
      </UCombobox>
      <p>Выбранный пользователь:</p>
      <div v-if="selectedUser" class="mt-2 text-gray-600 text-sm flex items-center gap-2">
        <img
          v-show="selectedUser.image"
          class="w-12 h-12 rounded-full"
          :src="selectedUser.image"
          :alt="selectedUser.lastName || ''"
        />
        {{ selectedUser.firstName }} {{ selectedUser.email || '' }}
      </div>
      <p class="mt-2 pt-2 border-t-2 border-disabled">
        пример со статичными данными, без фильтрации
      </p>
      <UCombobox
        v-model="selected"
        v-model:input="inputValue"
        :options="items"
        :display-value="(item: Item | null) => (item ? `${item.name}` : '')"
        class="w-full max-w-xs"
        @change="console.log('Selected:', $event)"
        show-toggle-button
      >
        <template #icon>
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M6 22C5.16667 22 4.45833 21.7083 3.875 21.125C3.29167 20.5417 3 19.8333 3 19V16H6V2L7.5 3.5L9 2L10.5 3.5L12 2L13.5 3.5L15 2L16.5 3.5L18 2L19.5 3.5L21 2V19C21 19.8333 20.7083 20.5417 20.125 21.125C19.5417 21.7083 18.8333 22 18 22H6ZM18 20C18.2833 20 18.5208 19.9042 18.7125 19.7125C18.9042 19.5208 19 19.2833 19 19V5H8V16H17V19C17 19.2833 17.0958 19.5208 17.2875 19.7125C17.4792 19.9042 17.7167 20 18 20ZM9 9V7H15V9H9ZM9 12V10H15V12H9ZM17 9C16.7167 9 16.4792 8.90417 16.2875 8.7125C16.0958 8.52083 16 8.28333 16 8C16 7.71667 16.0958 7.47917 16.2875 7.2875C16.4792 7.09583 16.7167 7 17 7C17.2833 7 17.5208 7.09583 17.7125 7.2875C17.9042 7.47917 18 7.71667 18 8C18 8.28333 17.9042 8.52083 17.7125 8.7125C17.5208 8.90417 17.2833 9 17 9ZM17 12C16.7167 12 16.4792 11.9042 16.2875 11.7125C16.0958 11.5208 16 11.2833 16 11C16 10.7167 16.0958 10.4792 16.2875 10.2875C16.4792 10.0958 16.7167 10 17 10C17.2833 10 17.5208 10.0958 17.7125 10.2875C17.9042 10.4792 18 10.7167 18 11C18 11.2833 17.9042 11.5208 17.7125 11.7125C17.5208 11.9042 17.2833 12 17 12Z"
              fill="#68CC7F"
            />
          </svg>
        </template>
      </UCombobox>
      <div v-if="selected" class="mt-2 text-gray-600 text-sm">Выбрано: {{ selected }}</div>
      <div class="text-sm text-gray-500">Текущее значение ввода: {{ inputValue }}</div>

      <p class="mt-4 border-t-2 border-disabled pt-2">
        пример с кастомной функцией фильтрации (фильтр по началу слова)
      </p>
      <UCombobox
        v-model="selectedFiltered"
        v-model:input="inputValueFiltered"
        :options="items"
        :display-value="(item: Item | null) => (item ? `${item.name}` : '')"
        :enable-filtering="true"
        :filter-function="customFilterFunction"
        class="w-full max-w-xs"
        placeholder="Введите начало названия фрукта"
        @change="console.log('Selected User:', $event)"
      />
    </main>
  </div>
</template>
