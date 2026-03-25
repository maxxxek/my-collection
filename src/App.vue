<template>
  <div class="container py-5">
    <h1 class="text-center mb-4 text-primary">Моя коллекция</h1>
    
    <AddItemForm @add="addItem" />
    <CollectionFilters 
      v-model:searchQuery="searchQuery"
      v-model:sortBy="sortBy"
      :currentFilter="currentFilter"
      @update:filter="currentFilter = $event"
    />
    
    <div v-if="filteredItems.length === 0" class="alert alert-info text-center">
      Нет элементов в коллекции
    </div>
    
    <div v-else class="row">
      <div 
        v-for="item in filteredItems" 
        :key="item.id" 
        class="col-md-4 mb-4"
      >
        <CollectionCard :item="item" @delete="deleteItem" />
      </div>
    </div>
    
    <div class="text-center mt-4 text-muted">
      Всего: {{ items.length }} элементов
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'
import AddItemForm from './components/AddItemForm.vue'
import CollectionFilters from './components/CollectionFilters.vue'
import CollectionCard from './components/CollectionCard.vue'

// Состояние коллекции
const items = ref([])

// Загрузка из localStorage
const saved = localStorage.getItem('collection')
if (saved) {
  items.value = JSON.parse(saved)
}

// Сохранение в localStorage
watch(items, (newItems) => {
  localStorage.setItem('collection', JSON.stringify(newItems))
}, { deep: true })

// Фильтры и сортировка
const searchQuery = ref('')
const currentFilter = ref('all')
const sortBy = ref('date')

// Добавление
const addItem = (newItem) => {
  items.value.push(newItem)
}

// Удаление
const deleteItem = (id) => {
  if (confirm('Удалить элемент?')) {
    items.value = items.value.filter(item => item.id !== id)
  }
}

// Фильтрация и сортировка
const filteredItems = computed(() => {
  let result = [...items.value]
  
  // Фильтрация
  result = result.filter(item => {
    if (currentFilter.value === 'with-description') {
      return item.description && item.description !== 'Без описания'
    }
    return true
  })
  
  // Поиск
  if (searchQuery.value) {
    const query = searchQuery.value.toLowerCase()
    result = result.filter(item => 
      item.title.toLowerCase().includes(query)
    )
  }
  
  // Сортировка
  if (sortBy.value === 'title') {
    result.sort((a, b) => a.title.localeCompare(b.title))
  } else if (sortBy.value === 'date') {
    result.sort((a, b) => b.id - a.id)
  }
  
  return result
})
</script>

<style>
body {
  background: #f8f9fa;
}
</style>