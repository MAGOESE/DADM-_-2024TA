<template>
  <div @click="selectPlan" class="plan">
    <div class="description">
      <span class="title">{{ name }} {{ selected ? '🌟' : '' }}</span>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

// Usando un macro para
// definir props
//defineProps(['name','planIcon']);
const props = defineProps({
  name: {
    type: String,
    required: true
  }
})
// Definiendo un evento
const emit = defineEmits(['select'])
// Variable que permite
// recordar si el plan ha sido seleccionado
const selected = ref(false)
// Funcion que permite cambiar el estado de la variables
//selected
const selectPlan = () => {
  selected.value = true
  // Emitir evento de que el
  // plan ha sido seleccionado
  emit('select', props.name)
}
</script>

<style scoped></style>