<script setup>
import { ref, onMounted } from 'vue'
import { apiClient } from '@/plugins/axios'
import TaskList from '@/components/task/TaskList.vue'

const tasks = ref([])
const errorMessage = ref('')

const fetchTasks = async () => {
  try {
    const response = await apiClient.get('/api/tasks')
    tasks.value = response.data.result
  } catch (error) {
    errorMessage.value = 'Error al obtener tareas'
    console.error(error)
  }
}

onMounted(fetchTasks)
</script>

<template>
  <div class="p-4">
    <h1 class="text-xl font-bold mb-4">Lista de Tareas</h1>

    <TaskList :tasks="tasks" />

    <p v-if="errorMessage" class="text-red-500">{{ errorMessage }}</p>
  </div>
</template>
