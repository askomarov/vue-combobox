<script setup lang="ts">
import { ref, onMounted } from 'vue'

const props = defineProps({
  owner: {
    type: String,
    required: true,
  },
  repo: {
    type: String,
    required: true,
  },
})

const stars = ref<number | null>(null)
const loading = ref(true)
const error = ref(false)

async function fetchGitHubStars() {
  const url = `https://api.github.com/repos/${props.owner}/${props.repo}`
  loading.value = true
  error.value = false

  try {
    const response = await fetch(url)
    if (!response.ok) throw new Error(`HTTP error! Status: ${response.status}`)

    const data = await response.json()
    stars.value = data.stargazers_count
  } catch (err) {
    console.error('Failed to fetch GitHub stars:', err)
    error.value = true
  } finally {
    loading.value = false
  }
}

onMounted(() => {
  fetchGitHubStars()
})
</script>

<template>
  <div class="flex items-center">
    <span class="flex items-center text-xs font-medium text-gray-600">
      <span v-if="loading">Loading...</span>
      <span v-else-if="error">-</span>
      <span v-else class="flex items-center space-x-1">
        <svg
          aria-hidden="true"
          height="16"
          viewBox="0 0 16 16"
          width="16"
          class="fill-current text-amber-500"
        >
          <path
            fill-rule="evenodd"
            d="M8 .25a.75.75 0 01.673.418l1.882 3.815 4.21.612a.75.75 0 01.416 1.279l-3.046 2.97.719 4.192a.75.75 0 01-1.088.791L8 12.347l-3.766 1.98a.75.75 0 01-1.088-.79l.72-4.194L.818 6.374a.75.75 0 01.416-1.28l4.21-.611L7.327.668A.75.75 0 018 .25z"
          ></path>
        </svg>
        <span>{{ stars?.toLocaleString() || '0' }}</span>
      </span>
    </span>
  </div>
</template>
