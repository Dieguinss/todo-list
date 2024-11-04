<template>
  <div id="app">
    <h1>Lista de Tarefas</h1>
    <input v-model="novaTarefa" @keyup.enter="adicionarTarefa" placeholder="Adicionar nova tarefa" />
    <button @click="adicionarTarefa">Adicionar</button>
    <ul>
      <li v-for="(tarefa, index) in tarefas" :key="index">
        <input 
          type="checkbox" 
          v-model="tarefa.concluida" 
          @change="marcarComoConcluida(index)" 
        />
        <span>
          {{ tarefa.texto }}
        </span>
        <span v-if="tarefa.concluida"> - Concluída em: {{ tarefa.dataConclusao }}</span>
        <button @click="removerTarefa(index)">Deletar</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      novaTarefa: '',
      tarefas: JSON.parse(localStorage.getItem('tarefas') || '[]')
    };
  },
  methods: {
    adicionarTarefa() {
      if (this.novaTarefa.trim() !== '') {
        this.tarefas.push({ 
          texto: this.novaTarefa, 
          concluida: false, 
          dataConclusao: null 
        });
        this.novaTarefa = '';
        this.salvarTarefas();
      }
    },
    marcarComoConcluida(index) {
      const tarefa = this.tarefas[index];
      if (tarefa.concluida) {
        tarefa.dataConclusao = new Date().toLocaleString();
      } else {
        tarefa.dataConclusao = null; // Limpa a data se for desmarcada, se necessário
      }
      this.salvarTarefas();
    },
    removerTarefa(index) {
      this.tarefas.splice(index, 1);
      this.salvarTarefas();
    },
    salvarTarefas() {
      localStorage.setItem('tarefas', JSON.stringify(this.tarefas));
    }
  }
};
</script>

<style>
/* Removendo o estilo de riscar */
#app {
  text-align: center;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 0;
}
</style>
