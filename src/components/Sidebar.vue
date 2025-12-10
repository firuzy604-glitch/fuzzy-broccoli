<template>
  <aside
    class="w-full md:w-64 bg-white border-r border-gray-100 p-6 md:min-h-screen md:fixed md:left-0 md:top-0 md:overflow-y-auto"
  >
    <h2 class="text-lg font-bold text-gray-900 mb-6 hidden md:block">Категории</h2>
    <nav class="space-y-2">
      <div v-for="category in mainCategories" :key="category.id" class="space-y-1">
        <!-- Main Category Button -->
        <button
          @click="toggleCategory(category.id)"
          :class="[
            'w-full text-left px-4 py-3 rounded-lg transition-all duration-200 font-medium flex items-center gap-3 justify-between',
            expandedCategories.includes(category.id)
              ? 'bg-gray-900 text-white shadow-md'
              : 'text-gray-700 hover:bg-gray-50',
          ]"
        >
          <span class="flex items-center gap-3 flex-1">
            <span class="text-lg">{{ category.icon }}</span>
            <span>{{ category.name }}</span>
          </span>
          <svg
            :class="[
              'w-5 h-5 transition-transform duration-200',
              expandedCategories.includes(category.id) ? 'rotate-180' : '',
            ]"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M19 14l-7 7m0 0l-7-7m7 7V3"
            />
          </svg>
        </button>

        <!-- Subcategories (collapsible) -->
        <transition
          enter-active-class="transition-all duration-200"
          leave-active-class="transition-all duration-200"
          enter-from-class="opacity-0 -translate-y-2 max-h-0"
          enter-to-class="opacity-100 translate-y-0 max-h-[500px]"
          leave-from-class="opacity-100 translate-y-0 max-h-[500px]"
          leave-to-class="opacity-0 -translate-y-2 max-h-0"
        >
          <div v-if="expandedCategories.includes(category.id)" class="overflow-hidden">
            <div class="pl-6 space-y-1 mt-1">
              <button
                v-for="subcategory in category.subcategories"
                :key="subcategory.id"
                @click="selectSubcategory(category.id, subcategory)"
                :class="[
                  'w-full text-left px-3 py-2 rounded-lg text-sm transition-all duration-200 flex items-center gap-2',
                  selectedSubcategoryId === subcategory.id
                    ? 'bg-gray-200 text-gray-900 font-semibold'
                    : 'text-gray-600 hover:text-gray-900 hover:bg-gray-100',
                ]"
              >
                <span
                  class="w-1 h-1 rounded-full"
                  :class="selectedSubcategoryId === subcategory.id ? 'bg-gray-900' : 'bg-gray-300'"
                />
                {{ subcategory.name }}
              </button>
            </div>
          </div>
        </transition>
      </div>
    </nav>
  </aside>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { mainCategories } from '@/data/categories'

interface Props {
  selectedSubcategoryId: string | null
}

const props = defineProps<Props>()

const expandedCategories = ref<number[]>([])

const emit = defineEmits<{
  'select-subcategory': [subcategoryId: string, categoryId: number]
}>()

const toggleCategory = (categoryId: number) => {
  if (expandedCategories.value.includes(categoryId)) {
    expandedCategories.value = expandedCategories.value.filter((id) => id !== categoryId)
  } else {
    expandedCategories.value.push(categoryId)
  }
}

const selectSubcategory = (categoryId: number, subcategory: { id: string; name: string }) => {
  emit('select-subcategory', subcategory.id, categoryId)
}
</script>
