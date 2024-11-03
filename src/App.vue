<template>
  <div id="app">
    <h1>Lista de Tarefas</h1>
    <input v-model="newTask" @keyup.enter="addTask" placeholder="Adicione uma nova tarefa" />
    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        {{ task }}
        <button @click="removeTask(index)">Remover</button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: []
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push(this.newTask.trim());
        this.newTask = '';
      }
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
    }
  }
};
</script>

<style>
#app {
  max-width: 600px;
  margin: auto;
  text-align: center;
}

input {
  padding: 10px;
  width: 80%;
  margin-bottom: 10px;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background: #f4f4f4;
  margin: 5px 0;
  padding: 10px;
}

button {
  background: red;
  color: white;
  border: none;
  cursor: pointer;
}
</style>
