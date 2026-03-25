<template>
  <form @submit.prevent="handleSubmit" class="mb-4">
    <div class="row g-2">
      <div class="col-md-5">
        <input
          v-model="form.title"
          type="text"
          class="form-control"
          placeholder="Название"
          required
        />
      </div>
      <div class="col-md-5">
        <input
          v-model="form.description"
          type="text"
          class="form-control"
          placeholder="Описание"
        />
      </div>
      <div class="col-md-2">
        <button type="submit" class="btn btn-primary w-100">
          Добавить
        </button>
      </div>
    </div>
  </form>
</template>

<script setup>
import { reactive } from 'vue'

const emit = defineEmits(['add'])

const form = reactive({
  title: '',
  description: ''
})

const handleSubmit = () => {
  if (form.title.trim()) {
    emit('add', {
      id: Date.now(),
      title: form.title.trim(),
      description: form.description.trim() || 'Без описания',
      category: 'Общее'
    })
    form.title = ''
    form.description = ''
  }
}
</script>