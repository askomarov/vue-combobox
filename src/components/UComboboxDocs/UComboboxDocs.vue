<script setup lang="ts">
import { ref } from 'vue'
import UCombobox from '../UCombobox/UCombobox.vue'

interface Item {
  id: string
  name: string
}

// Примеры для документации
const simpleItems = ref<Item[]>([
  { id: '1', name: 'Apple' },
  { id: '2', name: 'Banana' },
  { id: '3', name: 'Orange' },
  { id: '4', name: 'Mango' },
  { id: '5', name: 'Pineapple' },
])

// Базовый пример
const basicSelected = ref<Item | null>(null)
const basicInput = ref('')

// Пример с ошибкой
const errorSelected = ref<Item | null>(null)
const errorInput = ref('')
const errorExample = ref(true)

// Пример с загрузкой
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
// Пример с кастомным отображением
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

// Функция для отображения выбранного фрукта
const displayFruitValue = (item: Item | null) => {
  return item ? `${item.name}` : ''
}

// Пример с фильтрацией
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

// Пример с кастомной функцией фильтрации
const customFilterSelected = ref<Item | null>(null)
const customFilterInput = ref('')

// Пример кастомной функции фильтрации
const customFilterFunction = (item: Item, query: string) => {
  if (!query) return true
  return item.name.toLowerCase().startsWith(query.toLowerCase())
}
</script>

<template>
  <div class="u-combobox-docs">
    <h1 class="text-2xl font-bold mb-6">UCombobox - Документация</h1>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">Описание</h2>
      <p class="mb-4">
        UCombobox — это универсальный компонент для выбора из списка опций с поддержкой поиска.
        Компонент сочетает функциональность выпадающего списка (dropdown) и поля ввода (input),
        позволяя пользователям быстро находить и выбирать нужные элементы.
      </p>
      <p class="mb-4">Основные особенности:</p>
      <ul class="list-disc pl-6 mb-4">
        <li>Поддержка поиска по списку опций</li>
        <li>Поддержка клавиатурной навигации (стрелки вверх/вниз, Enter, Escape)</li>
        <li>Кастомизация отображения опций через слоты</li>
        <li>Поддержка асинхронной загрузки данных</li>
        <li>Обработка состояний загрузки и ошибок</li>
        <li>Возможность очистки выбранного значения</li>
        <li>Настраиваемое отображение выбранного значения</li>
        <li>Поддержка двусторонней привязки для выбранного значения и текста ввода</li>
      </ul>
    </section>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">Примеры использования</h2>

      <!-- Базовый пример -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Базовый пример</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="basicSelected"
            v-model:input="basicInput"
            :options="simpleItems"
            :display-value="displayFruitValue"
            placeholder="Выберите фрукт"
            show-toggle-button
          />
        </div>
        <div v-if="basicSelected" class="mb-2 text-sm">
          Выбрано: {{ basicSelected.name }} (ID: {{ basicSelected.id }})
        </div>
        <div class="mb-2 text-sm">Текущее значение поля ввода: {{ basicInput }}</div>
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

// Функция для отображения выбранного фрукта
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
    placeholder="Выберите фрукт"
    show-toggle-button
  /&gt;
&lt;/template&gt;</pre
        >
      </div>

      <!-- Пример с кастомным отображением -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Кастомное отображение опций через слоты</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="selectedUser"
            v-model:input="userInput"
            :options="users"
            :display-value="displayUserValue"
            placeholder="Выберите пользователя"
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
        <div class="mb-2 text-sm">Текущее значение поля ввода: {{ userInput }}</div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
&lt;UCombobox
  v-model="selectedUser"
  v-model:input="userInput"
  :options="users"
  :display-value="displayUserValue"
  placeholder="Выберите пользователя"
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

      <!-- Пример с состоянием загрузки -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Обработка состояния загрузки</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="loadingSelected"
            v-model:input="loadingInput"
            :display-value="(item) => (item ? item.name : '')"
            :is-loading="loadingExample"
            :options="[]"
            placeholder="Загрузка данных..."
          />
        </div>
        <button
          @click="loadingExample = !loadingExample"
          class="px-3 py-1 bg-accent-pink text-white rounded-md"
        >
          {{ loadingExample ? 'Остановить загрузку' : 'Показать загрузку' }}
        </button>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto mt-3">
&lt;UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :display-value="(item) => (item ? item.name : '')"
  :options="[]"
  :is-loading="isLoading"
  placeholder="Загрузка данных..."
/&gt;</pre
        >
      </div>

      <!-- Пример с ошибкой -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Обработка ошибок</h3>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="errorSelected"
            v-model:input="errorInput"
            :display-value="(item) => (item ? item.name : '')"
            :invalid="errorExample"
            :options="[]"
            :error="errorExample ? 'Не удалось загрузить данные' : null"
            placeholder="Пример с ошибкой"
          />
        </div>
        <button
          @click="errorExample = !errorExample"
          class="px-3 py-1 bg-accent-pink text-white rounded-md"
        >
          {{ errorExample ? 'Скрыть ошибку' : 'Показать ошибку' }}
        </button>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto mt-3">
&lt;UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :display-value="(item) => (item ? item.name : '')"
  :options="[]"
  :invalid="true"
  :error="'Не удалось загрузить данные'"
  placeholder="Пример с ошибкой"
/&gt;</pre
        >
      </div>

      <!-- Пример с фильтрацией -->
      <div class="example-block p-4 border rounded-lg mb-6 bg-gray-50">
        <h3 class="font-medium mb-2">Фильтрация локальных данных</h3>
        <p class="mb-4 text-sm text-gray-600">
          Компонент может автоматически фильтровать локальные данные при вводе текста в поле поиска.
          Это полезно, когда все данные доступны на клиенте и не требуется выполнять запросы к
          серверу.
        </p>

        <div class="flex items-center gap-2 mb-4">
          <label class="inline-flex items-center cursor-pointer">
            <input type="checkbox" v-model="filteringEnabled" class="sr-only peer" />
            <div
              class="relative w-11 h-6 bg-gray-200 peer-focus:outline-none peer-focus:ring-4 peer-focus:ring-blue-300 rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-5 after:w-5 after:transition-all peer-checked:bg-accent-pink"
            ></div>
            <span class="ml-3 text-sm font-medium">
              {{ filteringEnabled ? 'Фильтрация включена' : 'Фильтрация выключена' }}
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
            placeholder="Введите для фильтрации..."
            show-toggle-button
          />
        </div>
        <div class="mb-2 text-sm">Текущее значение поля ввода: {{ filteredInput }}</div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
&lt;UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :options="items"
  :display-value="displayFruitValue"
  :enable-filtering="true"
  placeholder="Введите для фильтрации..."
  show-toggle-button
/&gt;</pre
        >

        <h4 class="font-medium mb-2 mt-4">Кастомная функция фильтрации</h4>
        <p class="mb-4 text-sm text-gray-600">
          Вы можете определить собственную логику фильтрации, передав функцию в проп filterFunction.
        </p>
        <div class="mb-4 w-full max-w-xs">
          <UCombobox
            v-model="customFilterSelected"
            v-model:input="customFilterInput"
            :options="filterItems"
            :display-value="displayFruitValue"
            :enable-filtering="true"
            :filter-function="customFilterFunction"
            placeholder="Введите начало названия..."
          />
        </div>
        <pre class="bg-gray-800 text-white p-3 rounded-md text-sm overflow-x-auto">
        // Функция фильтрации по началу строки
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
          placeholder="Введите начало названия..."
        /&gt;</pre
        >
      </div>
    </section>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">API</h2>

      <h3 class="font-medium mb-2">Пропсы</h3>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse mb-6">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Имя</th>
              <th class="py-2 px-4 border text-left">Тип</th>
              <th class="py-2 px-4 border text-left">По умолчанию</th>
              <th class="py-2 px-4 border text-left">Описание</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">modelValue</td>
              <td class="py-2 px-4 border">T | null</td>
              <td class="py-2 px-4 border">null</td>
              <td class="py-2 px-4 border">Выбранное значение (v-model)</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">input</td>
              <td class="py-2 px-4 border">string</td>
              <td class="py-2 px-4 border">''</td>
              <td class="py-2 px-4 border">Значение поля ввода (v-model:input)</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">options</td>
              <td class="py-2 px-4 border">Array&lt;T&gt;</td>
              <td class="py-2 px-4 border">[]</td>
              <td class="py-2 px-4 border">Массив опций для выбора</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">displayValue</td>
              <td class="py-2 px-4 border">(item: T | null) => string</td>
              <td class="py-2 px-4 border">(item) => String(item)</td>
              <td class="py-2 px-4 border">Функция для отображения значения опции в поле ввода</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">isLoading</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Индикатор загрузки данных</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">error</td>
              <td class="py-2 px-4 border">string | null</td>
              <td class="py-2 px-4 border">null</td>
              <td class="py-2 px-4 border">Сообщение об ошибке</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">placeholder</td>
              <td class="py-2 px-4 border">string</td>
              <td class="py-2 px-4 border">'Выберите опцию...'</td>
              <td class="py-2 px-4 border">Плейсхолдер для поля ввода</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">class</td>
              <td class="py-2 px-4 border">string</td>
              <td class="py-2 px-4 border">''</td>
              <td class="py-2 px-4 border">CSS классы для корневого элемента</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">invalid</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Флаг невалидного состояния</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">showToggleButton</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">Показывать кнопку для открытия/закрытия списка</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">enableFiltering</td>
              <td class="py-2 px-4 border">boolean</td>
              <td class="py-2 px-4 border">false</td>
              <td class="py-2 px-4 border">
                Включает/выключает автоматическую фильтрацию локальных данных
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">filterFunction</td>
              <td class="py-2 px-4 border">(option: T, query: string) => boolean</td>
              <td class="py-2 px-4 border">undefined</td>
              <td class="py-2 px-4 border">Пользовательская функция фильтрации опций</td>
            </tr>
          </tbody>
        </table>
      </div>

      <h3 class="font-medium mb-2">События</h3>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse mb-6">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Имя</th>
              <th class="py-2 px-4 border text-left">Аргументы</th>
              <th class="py-2 px-4 border text-left">Описание</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">update:modelValue</td>
              <td class="py-2 px-4 border">(value: T | null)</td>
              <td class="py-2 px-4 border">Срабатывает при изменении выбранного значения</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">update:input</td>
              <td class="py-2 px-4 border">(value: string)</td>
              <td class="py-2 px-4 border">Срабатывает при изменении текста в поле ввода</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">change</td>
              <td class="py-2 px-4 border">(value: T | null)</td>
              <td class="py-2 px-4 border">Срабатывает при выборе опции</td>
            </tr>
          </tbody>
        </table>
      </div>

      <h3 class="font-medium mb-2">Слоты</h3>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Имя</th>
              <th class="py-2 px-4 border text-left">Параметры слота</th>
              <th class="py-2 px-4 border text-left">Описание</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">icon</td>
              <td class="py-2 px-4 border">-</td>
              <td class="py-2 px-4 border">Иконка в поле ввода</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">option-content</td>
              <td class="py-2 px-4 border">{ option }</td>
              <td class="py-2 px-4 border">Содержимое каждой опции в выпадающем списке</td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">empty</td>
              <td class="py-2 px-4 border">-</td>
              <td class="py-2 px-4 border">
                Содержимое, которое отображается, когда список опций пуст
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </section>

    <section class="mb-8">
      <h2 class="text-xl font-semibold mb-4">Клавиатурная навигация</h2>
      <p class="mb-4">Компонент поддерживает следующие комбинации клавиш:</p>
      <div class="overflow-x-auto">
        <table class="min-w-full border-collapse">
          <thead>
            <tr class="bg-gray-100">
              <th class="py-2 px-4 border text-left">Клавиша</th>
              <th class="py-2 px-4 border text-left">Действие</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td class="py-2 px-4 border">Стрелка вниз (↓)</td>
              <td class="py-2 px-4 border">
                Открывает выпадающий список, если он закрыт. Перемещает фокус на следующий элемент
                списка.
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">Стрелка вверх (↑)</td>
              <td class="py-2 px-4 border">
                Открывает выпадающий список, если он закрыт. Перемещает фокус на предыдущий элемент
                списка.
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">Enter</td>
              <td class="py-2 px-4 border">
                Выбирает элемент, на котором в данный момент находится фокус, и закрывает выпадающий
                список.
              </td>
            </tr>
            <tr>
              <td class="py-2 px-4 border">Escape</td>
              <td class="py-2 px-4 border">Закрывает выпадающий список.</td>
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
