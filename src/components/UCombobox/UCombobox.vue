<script setup lang="ts" generic="T">
import { ref, computed, watch, nextTick } from 'vue'
import { onClickOutside } from '@vueuse/core'
import { cn } from '@/lib/utils'

// Define interfaces
interface Props<T> {
  modelValue?: T | null
  input?: string
  options: T[]
  displayValue: (item: T | null) => string
  isLoading?: boolean
  error?: string | null
  placeholder?: string
  class?: string
  invalid?: boolean
  showToggleButton?: boolean
  enableFiltering?: boolean
  filterFunction?: (option: T, query: string) => boolean
}

// Define props with defaults
const props = withDefaults(defineProps<Props<T>>(), {
  modelValue: null,
  input: '',
  displayValue: (item: T | null) => (item ? String(item) : ''),
  isLoading: false,
  error: null,
  placeholder: 'Выберите опцию...',
  class: '',
  enableFiltering: false,
  filterFunction: undefined,
})

// Set default values for optional props
const defaultDisplayValue = (item: T | null) => (item ? String(item) : '')
const effectiveDisplayValue = props.displayValue || defaultDisplayValue

// Define emits
const emit = defineEmits<{
  (e: 'update:modelValue' | 'change', value: T | null): void
  (e: 'update:input', value: string): void
}>()

// State
const isOpen = ref(false)
const query = ref(props.input ?? '')
const activeIndex = ref<number | null>(null)
const inputElement = ref<HTMLInputElement | null>(null)
const comboboxElement = ref<HTMLElement | null>(null)

// Функция фильтрации по умолчанию
const defaultFilterFunction = (option: T, searchQuery: string): boolean => {
  if (!searchQuery.trim()) return true

  const optionText = props.displayValue(option).toLowerCase()
  return optionText.includes(searchQuery.toLowerCase())
}

// Отфильтрованные опции
const filteredOptions = computed(() => {
  if (!props.enableFiltering || !query.value) {
    return props.options
  }

  const filterFn = props.filterFunction || defaultFilterFunction
  return props.options.filter((option) => filterFn(option, query.value))
})

// Use VueUse's onClickOutside
onClickOutside(comboboxElement, () => {
  isOpen.value = false
  activeIndex.value = null
  const newQuery = props.displayValue(props.modelValue)
  query.value = newQuery
  emit('update:input', newQuery)
})

// Computed properties
const displayText = computed(() => props.displayValue(props.modelValue))

// Watch for modelValue changes to sync input
watch(
  () => props.modelValue,
  () => {
    const newQuery = displayText.value
    query.value = newQuery
    emit('update:input', newQuery)
  },
  { immediate: true },
)

// Watch for input prop changes to sync query
watch(
  () => props.input,
  (newInput) => {
    query.value = newInput ?? ''
  },
)

// Watch for options changes to reset active index
watch(
  () => props.options,
  () => {
    activeIndex.value = null
  },
)

// Methods
function toggle() {
  isOpen.value = !isOpen.value
  if (isOpen.value) {
    nextTick(() => inputElement.value?.focus())
  }
}

function selectOption(option: T | null) {
  emit('update:modelValue', option)
  emit('change', option)
  const newQuery = effectiveDisplayValue(option)
  query.value = newQuery
  emit('update:input', newQuery)
  isOpen.value = false
  activeIndex.value = null
}

function handleInput(event: Event) {
  const value = (event.target as HTMLInputElement).value
  query.value = value
  emit('update:input', value)
  isOpen.value = true
  if (!value && props.modelValue) {
    selectOption(null)
  }
}

function handleKeydown(event: KeyboardEvent) {
  if (!isOpen.value && (event.key === 'ArrowDown' || event.key === 'ArrowUp')) {
    isOpen.value = true
    return
  }

  if (event.key === 'ArrowDown') {
    event.preventDefault()
    activeIndex.value =
      activeIndex.value === null || activeIndex.value >= filteredOptions.value.length - 1
        ? 0
        : activeIndex.value + 1
  } else if (event.key === 'ArrowUp') {
    event.preventDefault()
    activeIndex.value =
      activeIndex.value === null || activeIndex.value <= 0
        ? filteredOptions.value.length - 1
        : activeIndex.value - 1
  } else if (event.key === 'Enter' && activeIndex.value !== null) {
    event.preventDefault()
    const selectedOption = filteredOptions.value[activeIndex.value] ?? null
    selectOption(selectedOption)
  } else if (event.key === 'Escape') {
    isOpen.value = false
    activeIndex.value = null
  }
}
</script>

<template>
  <div ref="comboboxElement" :class="cn('relative w-full', props.class)">
    <!-- Combobox Input and Button -->
    <div class="relative">
      <span class="absolute inset-y-0 start-4 flex items-center justify-center">
        <slot name="icon">
          <svg
            width="24"
            height="24"
            viewBox="0 0 24 24"
            fill="none"
            xmlns="http://www.w3.org/2000/svg"
          >
            <path
              d="M12 12C10.9 12 9.95833 11.6083 9.175 10.825C8.39167 10.0417 8 9.1 8 8C8 6.9 8.39167 5.95833 9.175 5.175C9.95833 4.39167 10.9 4 12 4C13.1 4 14.0417 4.39167 14.825 5.175C15.6083 5.95833 16 6.9 16 8C16 9.1 15.6083 10.0417 14.825 10.825C14.0417 11.6083 13.1 12 12 12ZM4 20V17.2C4 16.6333 4.14583 16.1125 4.4375 15.6375C4.72917 15.1625 5.11667 14.8 5.6 14.55C6.63333 14.0333 7.68333 13.6458 8.75 13.3875C9.81667 13.1292 10.9 13 12 13C13.1 13 14.1833 13.1292 15.25 13.3875C16.3167 13.6458 17.3667 14.0333 18.4 14.55C18.8833 14.8 19.2708 15.1625 19.5625 15.6375C19.8542 16.1125 20 16.6333 20 17.2V20H4Z"
              fill="currentColor"
            />
          </svg>
        </slot>
      </span>
      <input
        ref="inputElement"
        :value="query"
        type="text"
        role="combobox"
        :aria-expanded="isOpen"
        aria-autocomplete="list"
        class="selection:bg-primary selection:text-primary-foreground dark:bg-input/30 border-input-primary bg-input-primary active:bg-surface-primary placeholder:text-primary-disabled hover:bg-surface-primary hover:border-accent-pink focus:bg-surface-primary focus-visible:border-accent-pink aria-invalid:bg-bg-error aria-invalid:border-error flex min-h-[42px] w-full min-w-0 rounded-xl border py-[14px] pr-9 pl-11 text-sm leading-[19px] font-medium tracking-[0.015em] transition-[color,box-shadow] outline-none disabled:pointer-events-none disabled:cursor-not-allowed disabled:opacity-50"
        :placeholder="props.placeholder"
        :aria-invalid="props.invalid"
        @input="handleInput"
        @keydown="handleKeydown"
      />
      <button
        v-if="props.showToggleButton"
        @click="toggle"
        class="absolute inset-y-0 right-0 flex items-center px-2 text-gray-500"
        aria-label="Toggle options"
      >
        <svg
          class="w-5 h-5 transition-transform"
          :class="{ 'rotate-180': isOpen }"
          fill="none"
          stroke="currentColor"
          viewBox="0 0 24 24"
        >
          <path
            stroke-linecap="round"
            stroke-linejoin="round"
            stroke-width="2"
            d="M19 9l-7 7-7-7"
          />
        </svg>
      </button>
    </div>

    <!-- Options Dropdown -->
    <transition
      name="fade"
      enter-active-class="transition ease-out duration-100"
      enter-from-class="opacity-0 scale-90 translate-y-10"
      enter-to-class="opacity-100 scale-100 trasnslate-y-0"
      leave-active-class="transition ease-in duration-75"
      leave-from-class="opacity-100 scale-100 trasnslate-y-0"
      leave-to-class="opacity-0 scale-90 translate-y-10"
    >
      <ul
        v-if="isOpen"
        class="bg-graphic-primary absolute z-10 mt-2 grid max-h-[305px] w-full gap-3 overflow-auto rounded-2xl border-0 p-3 shadow-none"
        role="listbox"
      >
        <!-- Loading State -->
        <li
          v-if="props.isLoading"
          class="text-text-primary-inverted py-2 text-center text-sm text-[10px] font-semibold tracking-[0.015em]"
          role="option"
          aria-disabled="true"
        >
          Загрузка...
        </li>
        <!-- Error State -->
        <li
          v-else-if="props.error"
          class="px-4 py-2 text-center text-sm text-[10px] font-semibold tracking-[0.015em] text-red-600"
          role="option"
          aria-disabled="true"
        >
          {{ props.error }}
        </li>
        <!-- No Results State -->
        <li
          v-else-if="!props.isLoading && filteredOptions.length === 0"
          class="text-text-primary-inverted py-2 text-center text-sm text-[10px] font-semibold tracking-[0.015em]"
          role="option"
          aria-disabled="true"
        >
          <slot name="empty"> Кажется, ничего не найдено </slot>
        </li>
        <!-- Options List -->
        <li
          v-for="(option, index) in filteredOptions"
          :key="index"
          :class="[
            'text-text-primary-inverted flex cursor-pointer items-center justify-between gap-2 rounded-lg p-3 text-sm font-medium tracking-[0.015em]',
            index === activeIndex ? 'bg-disabled' : 'hover:bg-disabled',
          ]"
          role="option"
          :aria-selected="index === activeIndex"
          @click="selectOption(option)"
          @mouseenter="activeIndex = index"
        >
          <!-- Динамическое отображение данных в зависимости от типа -->
          <slot name="option-content" :option="option">
            <span>{{ props.displayValue(option) }}</span>
            <small
              class="text-text-tertiary text-[10px] font-semibold tracking-[0.015em] uppercase"
              >{{ option || '' }}</small
            >
          </slot>
        </li>
      </ul>
    </transition>
  </div>
</template>
