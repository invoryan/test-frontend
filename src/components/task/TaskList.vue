<script setup>
    import TaskItem from './TaskItem.vue'

    defineProps({
    tasks: Array
    })
</script>

<template>
  <div>
    <ul v-if="tasks.length">
      <TaskItem v-for="task in tasks" :key="task.id" :task="task" />
    </ul>
    <p v-else class="text-gray-500">No hay tareas aún.</p>
  </div>
</template>
