<script setup>
import { apiClient } from '@/plugins/axios'
import { ref, onMounted } from 'vue'
import { useRoute, useRouter } from 'vue-router'
import TaskForm from '@/components/task/TaskForm.vue'

const route = useRoute()
const router = useRouter()
const task = ref(null)
const errorMessage = ref(null)

onMounted(async () => {
  try {
    const response = await apiClient.get(`/api/tasks/${route.params.id}`)
    task.value = response.data.result
    console.log(task.value);
  } catch (error) {
    errorMessage.value = 'Error al cargar tarea'
  }
})

const updateTask = async taskData => {
  try {
    await apiClient.put(`/api/tasks/${route.params.id}`, taskData)
    router.push({ name: 'task-show' }) // Redirigir tras actualizar
  } catch (error) {
    errorMessage.value = 'Error al actualizar tarea'
  }
}
</script>

<template>
  <div>
    <h2>Editar Tarea</h2>
    <TaskForm v-if="task" :task="task" @submit="updateTask" />
    <p v-if="errorMessage">{{ errorMessage }}</p>
  </div>
</template>
