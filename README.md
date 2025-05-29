# [Vue Combobox](https://askomarov.github.io/vue-combobox/)

Универсальный компонент Combobox для Vue 3, реализованный с использованием TypeScript и TailwindCSS.

## Описание

UCombobox — это многофункциональный выпадающий список с возможностью поиска. Компонент сочетает функциональность выпадающего списка (dropdown) и поля ввода (input), позволяя пользователям быстро находить и выбирать нужные элементы.

### Возможности

- Поддержка поиска по списку опций
- Клавиатурная навигация (стрелки вверх/вниз, Enter, Escape)
- Кастомизация через слоты
- Асинхронная загрузка данных
- Обработка состояний загрузки и ошибок
- Фильтрация локальных данных
- Пользовательские функции фильтрации
- Полная типизация с использованием TypeScript

### Демо

Демонстрация работы компонента доступна при запуске проекта. Она включает:

- Базовый пример использования
- Кастомное отображение опций
- Асинхронная загрузка данных с API
- Работа с ошибками и состоянием загрузки
- Локальная фильтрация данных

## Установка и использование

### Установка зависимостей

```sh
pnpm install
```

### Запуск проекта для разработки

```sh
pnpm dev
```

### Сборка для продакшена

```sh
pnpm build
```

### Пример использования

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
    placeholder="Выберите фрукт"
    show-toggle-button
  />
</template>
```

### Локальная фильтрация

Компонент поддерживает автоматическую фильтрацию локальных данных:

```vue
<UCombobox
  v-model="selected"
  v-model:input="inputValue"
  :options="items"
  :display-value="displayValue"
  :enable-filtering="true"
  placeholder="Введите для поиска"
/>
```

## Документация

Подробная документация с примерами доступна в интерфейсе приложения при запуске проекта.

## Технический стек

- Vue 3
- TypeScript
- Tailwind CSS
- Vite
