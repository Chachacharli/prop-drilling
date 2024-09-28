<template>
  <section class="main-container">
    <h1>{{ user.name }}'s Task Manager</h1>
    <UserPanel />
    <TaskManager />
  </section>
</template>

<script setup>
import { ref, provide } from "vue";

import UserPanel from "../components/UserPanel.vue";
import TaskManager from "../components/TaskManager.vue";

// Objeto reactivo del usuario con una lista de tareas
const user = ref({
  name: "Jorgito",
  tasks: [
    { id: 1, description: "Learn Vue 3", completed: false },
    { id: 2, description: "Build a cool app", completed: false },
  ],
});

// Proveer el objeto `user` a los componentes descendientes
provide("user", user);
</script>


<style scoped>
.main-container{
  padding: 1rem;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  border: 1px solid rgb(41, 41, 36);
  border-radius: 1.5rem;
}

</style>