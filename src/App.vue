<script setup>
import { ref, computed } from 'vue';
import TarefaItem from './components/TarefaItem.vue';

const tarefas = ref([
  { id: 1, texto: 'Comprar leite', status: 'pendente' },
  { id: 2, texto: 'Estudar Vue.js', status: 'pendente' },
  { id: 3, texto: 'Fazer exercícios', status: 'pendente' }
]);

const novaTarefa = ref('');
const alteracao = ref(-1);
const filtro = ref('');
const tarefasExcluidas = ref([]);

function adicionarTarefa() {
  if (novaTarefa.value.trim().length >= 5) {
    if (alteracao.value >= 0) {
      tarefas.value[alteracao.value].texto = novaTarefa.value;
      alteracao.value = -1;
    }
    else {
      tarefas.value.push({
        id: Math.max(...tarefas.value.map(t => t.id)) + 1,
        texto: novaTarefa.value,
        status: 'pendente'
      });
    }
    novaTarefa.value = '';
  }
}

function excluirTarefa(id) {
  const posicao = tarefas.value.findIndex(t => t.id === id);
  tarefasExcluidas.value.push(tarefas.value[posicao]);
  tarefas.value.splice(posicao, 1);
}

function editarTarefa(id) {
  const posicao = tarefas.value.findIndex(t => t.id === id);
  novaTarefa.value = tarefas.value[posicao].texto;
  alteracao.value = posicao;
}

function marcarConcluida(id) {
  const posicao = tarefas.value.findIndex(t => t.id === id);
  tarefas.value[posicao].status = tarefas.value[posicao].status === 'pendente' ? 'concluida' : 'pendente';
}

function ordenarTarefas() {
  tarefas.value.sort((a, b) => a.texto.localeCompare(b.texto, 'pt-BR'));
}

function recuperarTarefa() {
  if (tarefasExcluidas.value.length > 0) {
    const tarefaRecuperada = tarefasExcluidas.value.pop();
    tarefas.value.push(tarefaRecuperada);
  }
}

const tarefasFiltradas = computed(() => {
  return tarefas.value.filter(t => t.texto.toLowerCase().includes(filtro.value.toLowerCase()));
});

const tarefasConcluidas = computed(() => {
  return tarefas.value.filter(t => t.status === 'concluida').length;
});
</script>

<template>
  <div class="container">
    <h1>To-Do List</h1>
    <form @submit.prevent="adicionarTarefa">
      <input
        v-model="novaTarefa"
        type="text"
        placeholder="Digite uma nova tarefa..."
        @keyup.enter="adicionarTarefa"
      />
      <button type="submit">{{ alteracao >= 0 ? 'Salvar' : 'Adicionar' }}</button>
    </form>
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
    <div>
      <p class="countDone">Tarefas concluídas: {{ tarefasConcluidas }} de {{ tarefas.length }}</p>
    </div>
    <div>
      <input type="text" v-model="filtro" placeholder="Filtrar tarefas..." />
    </div>
    <div>
      <button @click="ordenarTarefas">Ordenar</button>
      <button :disabled="tarefasExcluidas.length == 0" @click="recuperarTarefa">Recuperar Tarefa</button>
    </div>
  </div>
</template>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

form {
  margin-bottom: 1rem;
}

input {
  padding: 0.5rem;
  margin-right: 0.5rem;
}

button {
  padding: 0.5rem 1rem;
}

.countDone {
  font-weight: bold;
  font-size: smaller;
}
</style>
