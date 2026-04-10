<script setup>
defineProps(['temExcluidas'])
const emit = defineEmits(['ordenar','recuperar'])
</script>

<template>
  <div>
    <button @click="emit('ordenar')">Ordenar</button>

    <button 
      :disabled="temExcluidas == 0" 
      @click="emit('recuperar')"
    >
      Recuperar
    </button>
  </div>
</template>