<template>
  <div class="min-h-screen bg-gradient-to-br from-white to-gray-50">
    <Header @search="handleSearch" @sort="handleSort" />

    <div class="flex flex-col md:flex-row">
      <!-- Sidebar for desktop -->
      <div class="md:w-64 md:fixed md:left-0 md:top-20 md:min-h-screen">
        <Sidebar
          :selectedSubcategoryId="selectedSubcategoryId"
          @select-subcategory="handleSelectSubcategory"
        />
      </div>

      <!-- Main content -->
      <main class="flex-1 md:ml-64 p-6">
        <!-- Results header -->
        <div class="mb-8">
          <h2 class="text-xl font-bold text-gray-900 mb-2">
            {{ currentCategoryName }}
          </h2>
          <p class="text-gray-600">
            Показано {{ filteredAndSortedProducts.length }} из {{ products.length }} товаров
          </p>
        </div>

        <!-- Products grid -->
        <div
          v-if="filteredAndSortedProducts.length > 0"
          class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6"
        >
          <ProductCard
            v-for="product in filteredAndSortedProducts"
            :key="product.id"
            :product="product"
            @view-details="selectedProduct = product"
          />
        </div>

        <!-- Empty state -->
        <div v-else class="flex flex-col items-center justify-center py-16">
          <svg
            class="w-16 h-16 text-gray-300 mb-4"
            fill="none"
            stroke="currentColor"
            viewBox="0 0 24 24"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              stroke-width="2"
              d="M20 21l-4.35-4.35M11 19a8 8 0 100-16 8 8 0 000 16z"
            />
          </svg>
          <h3 class="text-xl font-semibold text-gray-700 mb-2">Товары не найдены</h3>
          <p class="text-gray-500">Попробуйте изменить поисковый запрос или фильтры</p>
        </div>
      </main>
    </div>

    <!-- Product Modal -->
    <ProductModal
      :product="selectedProduct"
      :isOpen="selectedProduct !== null"
      @close="selectedProduct = null"
    />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import Header from '@/components/Header.vue'
import Sidebar from '@/components/Sidebar.vue'
import ProductCard from '@/components/ProductCard.vue'
import { products } from '@/data/products'
import { mainCategories } from '@/data/categories'

const selectedSubcategoryId = ref<string | null>(null)
const selectedCategoryId = ref<number | null>(null)
const searchQuery = ref('')
const sortBy = ref('name')

const currentCategoryName = computed(() => {
  if (selectedSubcategoryId.value) {
    for (const category of mainCategories) {
      const subcategory = category.subcategories.find((s) => s.id === selectedSubcategoryId.value)
      if (subcategory) {
        return subcategory.name
      }
    }
  }

  if (selectedCategoryId.value) {
    const category = mainCategories.find((cat) => cat.id === selectedCategoryId.value)
    return category?.name || 'Все товары'
  }

  return 'Все товары'
})

const filteredAndSortedProducts = computed(() => {
  let result = [...products]

  // Filter by subcategory
  if (selectedSubcategoryId.value) {
    result = result.filter((p) => p.subcategoryId === selectedSubcategoryId.value)
  }

  // Filter by search query
  if (searchQuery.value.trim()) {
    const query = searchQuery.value.toLowerCase()
    result = result.filter(
      (p) =>
        p.name.toLowerCase().includes(query) ||
        p.description.toLowerCase().includes(query) ||
        p.category.toLowerCase().includes(query) ||
        p.subcategory.toLowerCase().includes(query),
    )
  }

  // Sort
  if (sortBy.value === 'name') {
    result.sort((a, b) => a.name.localeCompare(b.name))
  } else if (sortBy.value === 'price-low') {
    result.sort((a, b) => a.price - b.price)
  } else if (sortBy.value === 'price-high') {
    result.sort((a, b) => b.price - a.price)
  } else if (sortBy.value === 'rating') {
    result.sort((a, b) => b.rating - a.rating)
  }

  return result
})

const handleSearch = (query: string) => {
  searchQuery.value = query
}

const handleSort = (type: string) => {
  sortBy.value = type
}

const handleSelectSubcategory = (subcategoryId: string, categoryId: number) => {
  selectedSubcategoryId.value = subcategoryId
  selectedCategoryId.value = categoryId
}
</script>
