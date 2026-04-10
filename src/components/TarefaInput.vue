<script setup>
const props = defineProps(['texto','alteracao'])
const emit = defineEmits(['salvar','update:texto'])

function salvar() {
  emit('salvar', props.texto)
}
</script>

<template>
  <form @submit.prevent="salvar">
    <input
      :value="texto"
      @input="emit('update:texto', $event.target.value)"
      placeholder="Digite uma tarefa..."
    />
    <button>
      {{ alteracao >= 0 ? 'Salvar' : 'Adicionar' }}
    </button>
  </form>
</template>