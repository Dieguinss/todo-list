<template>
  <div :class="theme" id="app">
    <h1>Lista de Tarefas</h1>
    <input type="text" v-model="newTask" placeholder="Nova Tarefa" />
    <button @click="addTask">Adicionar</button>

    <div>
      <select v-model="sortOrder" @change="sortTasks">
        <option value="creation">Ordem de Criação</option>
        <option value="alphabetical">Ordem Alfabética</option>
      </select>
      <button @click="completeAllTasks">Marcar Todas Como Concluídas</button>
      <button @click="toggleTheme">Alternar Tema</button>
    </div>

    <transition-group name="task" tag="ul">
      <li v-for="task in tasks" :key="task.id" :class="{ completed: task.completed }">
        <input type="checkbox" v-model="task.completed" @change="toggleTask(task)" :disabled="task.completed" />
        <span>{{ task.name }}</span>
        <button @click="deleteTask(task)">Deletar</button>
        <span v-if="task.completed">Concluído em {{ task.completedAt }}</span>
      </li>
    </transition-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTask: "",
      tasks: JSON.parse(localStorage.getItem("tasks")) || [],
      theme: "light",
      sortOrder: "creation"
    };
  },
  methods: {
    addTask() {
      if (!this.newTask.trim()) {
        alert("A tarefa não pode estar vazia!");
        return;
      }
      if (this.tasks.some(task => task.name === this.newTask)) {
        alert("A tarefa já existe!");
        return;
      }
      const task = {
        id: Date.now(),
        name: this.newTask,
        completed: false,
        completedAt: null
      };
      this.tasks.push(task);
      this.saveTasks();
      this.newTask = "";
    },
    deleteTask(task) {
      this.tasks = this.tasks.filter(t => t.id !== task.id);
      this.saveTasks();
    },
    toggleTask(task) {
      if (!task.completed) {
        task.completed = true;
        task.completedAt = new Date().toLocaleString();
        this.saveTasks();
      }
    },
    completeAllTasks() {
      this.tasks.forEach(task => {
        task.completed = true;
        task.completedAt = new Date().toLocaleString();
      });
      this.saveTasks();
    },
    sortTasks() {
      if (this.sortOrder === "alphabetical") {
        this.tasks.sort((a, b) => a.name.localeCompare(b.name));
      } else {
        this.tasks.sort((a, b) => a.id - b.id);
      }
    },
    toggleTheme() {
      this.theme = this.theme === "light" ? "dark" : "light";
    },
    saveTasks() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    }
  },
};
</script>

<style>
#app {
  padding: 20px;
}

.task-enter-active, .task-leave-active {
  transition: opacity 0.5s;
}
.task-enter, .task-leave-to {
  opacity: 0;
}

.completed {
  color: green;
}

.light {
  background-color: white;
  color: black;
}
.dark {
  background-color: black;
  color: white;
}
</style>
