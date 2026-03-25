<template>
  <div class="card h-100 shadow-sm">
    <div class="card-body">
      <h5 class="card-title">{{ item.title }}</h5>
      <p v-if="item.description" class="card-text text-muted">
        {{ item.description }}
      </p>
      <p v-if="item.category" class="badge bg-secondary">{{ item.category }}</p>
    </div>
    <div class="card-footer bg-transparent d-flex justify-content-between">
      <button @click="$emit('delete', item.id)" class="btn btn-sm btn-danger">
        Удалить
      </button>
    </div>
  </div>
</template>

<script setup>
defineProps(['item'])
defineEmits(['delete'])
</script>