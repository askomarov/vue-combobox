<script setup lang="ts">
import GithubIcon from '@/assets/github.svg?component'
import GithubStar from '@/components/GithubStar/GithubStar.vue'
</script>

<template>
  <header class="py-4 mb-6">
    <div class="max-w-7xl mx-auto px-4">
      <div class="flex justify-between items-center">
        <h2 class="text-2xl font-bold text-accent-pink">UCombobox</h2>
        <div class="flex items-center space-x-4">
          <a
            href="https://github.com/askomarov/vue-combobox"
            target="_blank"
            rel="noopener noreferrer"
            class="flex items-center justify-center text-gray-700 hover:text-accent-pink transition-colors duration-200 gap-2"
            title="Исходный код на GitHub"
          >
            <GithubStar owner="askomarov" repo="vue-combobox" />
            <span class="sr-only">GitHub</span>
            <GithubIcon class="fill-current" />
          </a>
          <!-- <iframe
            src="https://ghbtns.com/github-btn.html?user=askomarov&repo=vue-combobox&type=star&count=true"
            frameborder="0"
            scrolling="0"
            width="150"
            height="20"
            title="GitHub"
          ></iframe> -->
        </div>
      </div>
    </div>
  </header>
</template>
