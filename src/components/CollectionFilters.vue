<template>
  <div class="row mb-4">
    <div class="col-md-6">
      <input
        v-model="searchQuery"
        type="text"
        class="form-control"
        placeholder="Поиск по названию..."
      />
    </div>
    <div class="col-md-3">
      <select v-model="sortBy" class="form-select">
        <option value="title">Сортировать по названию</option>
        <option value="date">Сортировать по дате (новые)</option>
      </select>
    </div>
    <div class="col-md-3">
      <div class="btn-group w-100">
        <button
          v-for="filter in filters"
          :key="filter.value"
          @click="$emit('update:filter', filter.value)"
          :class="['btn', filter.value === currentFilter ? 'btn-primary' : 'btn-outline-secondary']"
        >
          {{ filter.label }}
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps(['currentFilter', 'searchQuery', 'sortBy'])
defineEmits(['update:filter', 'update:searchQuery', 'update:sortBy'])

const filters = [
  { value: 'all', label: 'Все' },
  { value: 'with-description', label: 'С описанием' }
]

const searchQuery = defineModel('searchQuery')
const sortBy = defineModel('sortBy')
</script>