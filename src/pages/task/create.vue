<script setup>
    import { apiClient } from '@/plugins/axios'
    import TaskForm from '@/components/task/TaskForm.vue'

    const createTask = async (formattedData) => {
    try {
        const response = await apiClient.post('/api/tasks', formattedData)
        console.log('Tarea creada correctamente:', response.data)
    } catch (error) {
        console.error('Error al crear la tarea:', error)
    }
    }
</script>

<template>
  <TaskForm @submit="createTask" />
</template>

  