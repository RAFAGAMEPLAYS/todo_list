<script setup>
defineProps(['filtro'])
const emit = defineEmits(['update:filtro'])
</script>

<template>
  <input
    :value="filtro"
    @input="emit('update:filtro', $event.target.value)"
    placeholder="Filtrar tarefas..."
  />
</template>