<script setup>
    import { ref, defineEmits } from 'vue'

    const emit = defineEmits(['submit'])
    const form = ref({
        title: '',
        description: '',
        due_date: '',
        status: 'PENDIENTE',
    })

    const rules = {
    required: value => !!value || 'Este campo es obligatorio',
    }

    const formatData = () => {
        return {
            title: form.value.title,
            description: form.value.description,
            due_date: form.value.due_date,
            status: form.value.status
        }
    }

    const submitForm = () => {
    emit('submit', formatData())
    }
</script>
<template>
    <VForm @submit.prevent="submitForm">
      <VTextField v-model="form.title" label="Título" :rules="[rules.required]" /> <br>
      <VTextField v-model="form.description" label="Descripción" :rules="[rules.required]" /><br>
      <VTextField v-model="form.due_date" label="Fecha límite" type="date" :rules="[rules.required]" /><br>
      
      <VSelect
        v-model="form.status"
        :items="['PENDIENTE', 'OBSERVADO', 'COMPLETADO']"
        label="Estado"
        :rules="[rules.required]"
      /><br>
  
      <VBtn type="submit" color="primary">Guardar</VBtn>
    </VForm>
</template>
