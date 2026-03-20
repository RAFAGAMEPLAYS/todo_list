<template>
  <div>
    <h1>To-Do List</h1>

   
    <input v-model="novaTarefa" placeholder="Digite uma tarefa" />
    <button @click="adicionar">Adicionar</button>

    
    <div>
      <button @click="ordenarAZ">A-Z</button>
      <button @click="ordenarZA">Z-A</button>
    </div>

   
    <ul>
      <li v-for="(tarefa, index) in tarefas" :key="index">

        <!-- CHECK -->
        <input type="checkbox" v-model="tarefa.concluido" />

       
        <span v-if="editandoIndex !== index"
              :style="{ textDecoration: tarefa.concluido ? 'line-through' : 'none' }">
          {{ tarefa.texto }}
        </span>

        <input v-else v-model="tarefa.texto" />

        
        <button @click="editar(index)">
          {{ editandoIndex === index ? 'Salvar' : 'Editar' }}
        </button>

        <button @click="remover(index)">deletar</button>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const novaTarefa = ref("")
const tarefas = ref([])
const editandoIndex = ref(null)


function adicionar() {
  if (novaTarefa.value.trim() === "") return

  tarefas.value.push({
    texto: novaTarefa.value,
    concluido: false
  })

  novaTarefa.value = ""
}


function remover(index) {
  tarefas.value.splice(index, 1)
}


function editar(index) {
  if (editandoIndex.value === index) {
    editandoIndex.value = null
  } else {
    editandoIndex.value = index
  }
}


function ordenarAZ() {
  tarefas.value.sort((a, b) => a.texto.localeCompare(b.texto))
}

function ordenarZA() {
  tarefas.value.sort((a, b) => b.texto.localeCompare(a.texto))
}
</script>