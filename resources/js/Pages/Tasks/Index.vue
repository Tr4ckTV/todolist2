<template>
    <div>
      <h1>Liste de tâches</h1>
  
      <ul>
        <li v-for="task in tasks" :key="task.id">
          <input
            type="checkbox"
            v-model="task.completed"
            @change="updateTask(task)"
          />
          <span :class="{ completed: task.completed }">{{ task.title }}</span>
          <button @click="deleteTask(task)">Supprimer</button>
        </li>
      </ul>
  
      <form @submit.prevent="addTask">
        <input v-model="newTask" placeholder="Nouvelle tâche..." />
        <button type="submit">Ajouter</button>
      </form>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      tasks: Array,
    },
    data() {
      return {
        newTask: '',
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() === '') return;
  
        this.$inertia.post('/tasks', {
          title: this.newTask,
        });
  
        this.newTask = '';
      },
      updateTask(task) {
        this.$inertia.put(`/tasks/${task.id}`, {});
      },
      deleteTask(task) {
        if (confirm('Êtes-vous sûr de vouloir supprimer cette tâche ?')) {
          this.$inertia.delete(`/tasks/${task.id}`);
        }
      },
    },
  };
  </script>
  
  <style scoped>
  .completed {
    text-decoration: line-through;
  }
  </style>
  