<script setup>
import { ref, watch, defineEmits, defineProps } from 'vue'

const emit = defineEmits(['submit'])

const props = defineProps({
  task: Object
})

const form = ref({
  title: '',
  description: '',
  due_date: '',
  status: 'PENDIENTE',
})

watch(() => props.task, (newTask) => {
  if (newTask) {
    form.value = { ...newTask }
  }
}, { immediate: true })

const rules = {
  required: value => !!value || 'Este campo es obligatorio',
}

const submitForm = () => {
  emit('submit', form.value)
}
</script>

<template>
  <VForm @submit.prevent="submitForm">
    <br><VTextField v-model="form.title" label="Título" :rules="[rules.required]" required /><br>
    
    <VTextField v-model="form.description" label="Descripción" :rules="[rules.required]" required /><br>
    
    <VTextField v-model="form.due_date" label="Fecha límite" type="date" :rules="[rules.required]" required /><br>
    
    <VSelect
      v-model="form.status"
      :items="['PENDIENTE','OBSERVADO', 'COMPLETADO']"
      label="Estado"
      :rules="[rules.required]"
      required
    />

    <br><VBtn type="submit" color="primary">Guardar</VBtn>
  </VForm>
</template>

<style scoped>
</style>
