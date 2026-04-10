<script setup>
defineProps(['id', 'texto', 'status']);
const emit = defineEmits(['marcarConcluida', 'excluirTarefa', 'editarTarefa']);
</script>

<template>
  <li>
    <span id="task" :class="{ concluida: status === 'concluida' }" @click="emit('marcarConcluida', id)">
      {{ texto }}
    </span>
    <a href="#" style="color: red;" @click.prevent="emit('excluirTarefa', id)">Excluir</a>
    <a href="#" @click.prevent="emit('editarTarefa', id)">Editar</a>
  </li>
</template>

<style scoped>

</style>