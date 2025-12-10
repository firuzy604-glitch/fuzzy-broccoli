<template>
  <transition
    enter-active-class="transition-opacity duration-300"
    leave-active-class="transition-opacity duration-300"
    enter-from-class="opacity-0"
    leave-to-class="opacity-0"
  >
    <div
      v-if="isOpen"
      class="fixed inset-0 z-50 overflow-y-auto bg-black bg-opacity-50 flex items-center justify-center p-4"
      @click="closeModal"
    >
      <transition
        enter-active-class="transition-all duration-300"
        leave-active-class="transition-all duration-300"
        enter-from-class="opacity-0 scale-95"
        leave-to-class="opacity-0 scale-95"
      >
        <div
          v-if="isOpen"
          class="bg-white rounded-2xl shadow-2xl max-w-2xl w-full max-h-[90vh] overflow-y-auto"
          @click.stop
        >
          <!-- Close Button -->
          <button
            @click="closeModal"
            class="absolute top-4 right-4 p-2 rounded-full bg-gray-100 hover:bg-gray-200 transition-colors z-10"
          >
            <svg
              class="w-6 h-6 text-gray-600"
              fill="none"
              stroke="currentColor"
              viewBox="0 0 24 24"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="M6 18L18 6M6 6l12 12"
              />
            </svg>
          </button>

          <!-- Product Image -->
          <div class="relative w-full h-80 overflow-hidden bg-gray-100">
            <img :src="product.image" :alt="product.name" class="w-full h-full object-cover" />
            <div
              class="absolute top-4 left-4 bg-gray-900 text-white px-4 py-2 rounded-full text-sm font-medium"
            >
              {{ product.subcategory }}
            </div>
          </div>

          <!-- Product Details -->
          <div class="p-8">
            <!-- Title and Price -->
            <div class="mb-6">
              <h2 class="text-3xl font-bold text-gray-900 mb-4">{{ product.name }}</h2>
              <div class="flex items-baseline gap-4">
                <span class="text-4xl font-bold text-gray-900"
                  >${{ product.price.toFixed(2) }}</span
                >
                <div class="flex items-center gap-2">
                  <span class="text-yellow-400 text-lg">★</span>
                  <span class="text-lg font-semibold text-gray-900">{{ product.rating }}</span>
                  <span class="text-gray-500">({{ product.reviews }} отзывов)</span>
                </div>
              </div>
            </div>

            <!-- Description -->
            <div class="mb-8">
              <p class="text-gray-700 text-lg leading-relaxed">{{ product.description }}</p>
            </div>

            <!-- Ingredients/Composition -->
            <div class="mb-8 border-t border-b border-gray-200 py-6">
              <h3 class="text-xl font-bold text-gray-900 mb-4">Состав</h3>
              <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                <div
                  v-for="(ingredient, index) in product.ingredients"
                  :key="index"
                  class="flex items-start gap-3"
                >
                  <span class="text-gray-400 font-bold">•</span>
                  <span class="text-gray-700">{{ ingredient }}</span>
                </div>
              </div>
            </div>

            <!-- Specifications -->
            <div v-if="product.specifications" class="mb-8">
              <h3 class="text-xl font-bold text-gray-900 mb-4">Технические характеристики</h3>
              <div class="space-y-3">
                <div
                  v-for="(spec, index) in product.specifications"
                  :key="index"
                  class="flex justify-between py-2 border-b border-gray-100 last:border-b-0"
                >
                  <span class="text-gray-600">{{ spec.split(':')[0] }}</span>
                  <span class="text-gray-900 font-semibold">{{ spec.split(':')[1] }}</span>
                </div>
              </div>
            </div>

            <!-- Category Info -->
            <div class="grid grid-cols-2 gap-4 mb-8 bg-gray-50 p-4 rounded-lg">
              <div>
                <p class="text-sm text-gray-600 mb-1">Главная категория</p>
                <p class="text-lg font-semibold text-gray-900">{{ product.category }}</p>
              </div>
              <div>
                <p class="text-sm text-gray-600 mb-1">Подкатегория</p>
                <p class="text-lg font-semibold text-gray-900">{{ product.subcategory }}</p>
              </div>
            </div>

            <!-- Action Buttons -->
            <div class="flex gap-4 sticky bottom-0 bg-white pt-4 border-t">
              <button
                @click="closeModal"
                class="flex-1 px-6 py-3 border-2 border-gray-900 text-gray-900 font-bold rounded-lg hover:bg-gray-50 transition-colors"
              >
                Закрыть
              </button>
              <button
                class="flex-1 px-6 py-3 bg-gray-900 text-white font-bold rounded-lg hover:bg-gray-800 transition-colors"
              >
                Добавить в корзину
              </button>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </transition>
</template>

<script setup lang="ts">
import type { Product } from '@/data/products'

interface Props {
  product: Product | null
  isOpen: boolean
}

defineProps<Props>()

const emit = defineEmits<{
  close: []
}>()

const closeModal = () => {
  emit('close')
}
</script>
