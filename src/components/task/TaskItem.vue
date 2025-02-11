<script setup>
    import { apiClient } from '@/plugins/axios'
    import { ref } from 'vue'
    import { useRouter } from 'vue-router'

    const props = defineProps({
    task: Object
    })

    const router = useRouter()
    const deleting = ref(false)

    const deleteTask = async () => {
    if (!confirm('¿Seguro que quieres eliminar esta tarea?')) return

    deleting.value = true
    try {
        await apiClient.delete(`/api/tasks/${props.task.id}`)
        alert('Tarea eliminada correctamente')
        window.location.reload()
    } catch (error) {
        alert('Error al eliminar tarea')
        console.error(error)
    } finally {
        deleting.value = false
    }
    }
</script>

<template>
  <li class="border p-3 mb-2 rounded flex justify-between items-center">
    <span>{{ task.title }}</span>
    
    <div class="space-x-2">
      <button @click="router.push(`/task/${task.id}/edit`)" class="bg-yellow-500 text-white px-3 py-1 rounded">
        Editar
      </button>
      
      <button @click="deleteTask" class="bg-red-500 text-white px-3 py-1 rounded" :disabled="deleting">
        {{ deleting ? 'Eliminando...' : 'Eliminar' }}
      </button>
    </div>
  </li>
</template>

