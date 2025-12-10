<template>
  <button
    @click="emit('view-details')"
    class="bg-white rounded-xl overflow-hidden shadow-md hover:shadow-xl transition-all duration-300 group text-left w-full"
  >
    <div class="relative h-48 overflow-hidden bg-gray-100">
      <img
        :src="product.image"
        :alt="product.name"
        class="w-full h-full object-cover group-hover:scale-105 transition-transform duration-300"
      />
      <div
        class="absolute top-3 right-3 bg-gray-900 text-white px-3 py-1 rounded-full text-sm font-medium"
      >
        {{ product.subcategory }}
      </div>
    </div>
    <div class="p-4">
      <h3 class="font-bold text-gray-900 mb-2 line-clamp-2">
        {{ product.name }}
      </h3>
      <p class="text-sm text-gray-600 mb-3 line-clamp-2">
        {{ product.description }}
      </p>
      <div class="flex items-center justify-between mb-3">
        <div class="flex items-center gap-1">
          <span class="text-yellow-400">★</span>
          <span class="text-sm font-semibold text-gray-900">{{ product.rating }}</span>
          <span class="text-xs text-gray-500">({{ product.reviews }})</span>
        </div>
      </div>
      <div class="flex items-center justify-between">
        <span class="text-xl font-bold text-gray-900">${{ product.price.toFixed(2) }}</span>
        <div
          class="bg-gray-900 text-white px-4 py-2 rounded-lg hover:bg-gray-800 transition-colors font-medium text-sm"
        >
          Подробнее
        </div>
      </div>
    </div>
  </button>
</template>

<script setup lang="ts">
import type { Product } from '@/data/products'

defineProps<{
  product: Product
}>()

const emit = defineEmits<{
  'view-details': []
}>()
</script>
