<script setup>
import { ref, computed } from 'vue'
import TarefaItem from './components/TarefaItem.vue'
import TarefaInput from './components/TarefaInput.vue'
import TarefaFiltro from './components/TarefaFiltro.vue'
import TarefaAcoes from './components/TarefaAcoes.vue'

const tarefas = ref([
  { id: 1, texto: 'farmar aura', status: 'pendente' },
  { id: 2, texto: 'jogar roblox', status: 'pendente' },
  { id: 3, texto: 'ser tipo o ayanokoji', status: 'pendente' }
])

const novaTarefa = ref('')
const alteracao = ref(-1)
const filtro = ref('')
const tarefasExcluidas = ref([])

function adicionarTarefa(texto) {
  if (texto.trim().length >= 5) {
    if (alteracao.value >= 0) {
      tarefas.value[alteracao.value].texto = texto
      alteracao.value = -1
    } else {
      tarefas.value.push({
        id: Date.now(),
        texto,
        status: 'pendente'
      })
    }
    novaTarefa.value = ''
  }
}

function excluirTarefa(id) {
  const pos = tarefas.value.findIndex(t => t.id === id)
  tarefasExcluidas.value.push(tarefas.value[pos])
  tarefas.value.splice(pos, 1)
}

function editarTarefa(id) {
  const pos = tarefas.value.findIndex(t => t.id === id)
  novaTarefa.value = tarefas.value[pos].texto
  alteracao.value = pos
}

function marcarConcluida(id) {
  const t = tarefas.value.find(t => t.id === id)
  t.status = t.status === 'pendente' ? 'concluida' : 'pendente'
}

function ordenarTarefas() {
  tarefas.value.sort((a, b) => a.texto.localeCompare(b.texto))
}

function recuperarTarefa() {
  if (tarefasExcluidas.value.length > 0) {
    tarefas.value.push(tarefasExcluidas.value.pop())
  }
}

const tarefasFiltradas = computed(() =>
  tarefas.value.filter(t =>
    t.texto.toLowerCase().includes(filtro.value.toLowerCase())
  )
)

const tarefasConcluidas = computed(() =>
  tarefas.value.filter(t => t.status === 'concluida').length
)
</script>

<template>
  <div class="container">
    <h1 class="sigma">Lista legal de Tarefas</h1>

    <TarefaInput
      :texto="novaTarefa"
      :alteracao="alteracao"
      @salvar="adicionarTarefa"
      @update:texto="novaTarefa = $event"
    />

    <ul>
      <TarefaItem
        v-for="tarefa in tarefasFiltradas"
        :key="tarefa.id"
        :id="tarefa.id"
        :texto="tarefa.texto"
        :status="tarefa.status"
        @marcarConcluida="marcarConcluida"
        @excluirTarefa="excluirTarefa"
        @editarTarefa="editarTarefa"
      />
    </ul>

    <p class="countDone">
      {{ tarefasConcluidas }} / {{ tarefas.length }}
    </p>

    <TarefaFiltro
      :filtro="filtro"
      @update:filtro="filtro = $event"
    />

    <TarefaAcoes
      :temExcluidas="tarefasExcluidas.length"
      @ordenar="ordenarTarefas"
      @recuperar="recuperarTarefa"
    />
  </div>
</template>
<style scoped>
.sigma{
  
  font-size: 48px;
  font-weight: bold;
  background: linear-gradient(to right, #30cfd0, #330867);
  
  -webkit-background-clip: text;
  background-clip: text;
  
  -webkit-text-fill-color: transparent;
  
  display: inline-block;

}
</style>
