<template>
  <header class="bg-white border-b border-gray-100 sticky top-0 z-40">
    <div class="px-6 py-4">
      <div class="flex flex-col md:flex-row md:items-center md:justify-between gap-4">
        <h1 class="text-2xl md:text-3xl font-bold text-gray-900">
          <span class="text-gray-400">Kitchen</span> Essentials
        </h1>
        <div class="flex flex-col sm:flex-row gap-3 flex-1 md:flex-none md:max-w-md">
          <div class="relative flex-1">
            <input
              v-model="searchQuery"
              @input="$emit('search', searchQuery)"
              type="text"
              placeholder="Search products..."
              class="w-full px-4 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-900 focus:border-transparent"
            />
            <svg
              class="absolute right-3 top-1/2 transform -translate-y-1/2 text-gray-400 w-5 h-5"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"
              />
            </svg>
          </div>
          <select
            v-model="sortBy"
            @change="$emit('sort', sortBy)"
            class="px-4 py-2 border border-gray-200 rounded-lg focus:outline-none focus:ring-2 focus:ring-gray-900 focus:border-transparent text-gray-700"
          >
            <option value="name">Sort by Name</option>
            <option value="price-low">Price: Low to High</option>
            <option value="price-high">Price: High to Low</option>
            <option value="rating">Top Rated</option>
          </select>
        </div>
      </div>
    </div>
  </header>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const searchQuery = ref('')
const sortBy = ref('name')

defineEmits<{
  search: [query: string]
  sort: [type: string]
}>()
</script>
