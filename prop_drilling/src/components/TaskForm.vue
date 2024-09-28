<template>
  <div>
    <h3>Add New Task</h3>
    <input v-model="newTaskDescription" placeholder="New task" />
    <button @click="addTask">Add Task</button>
  </div>
</template>

<script setup>
import { inject, ref } from "vue";

// Inyectar el objeto `user`
const user = inject("user");

// Variable para almacenar la descripción de la nueva tarea
const newTaskDescription = ref("");

// Función para agregar una nueva tarea al objeto `user`
const addTask = () => {
  if (newTaskDescription.value.trim() !== "") {
    user.value.tasks.push({
      id: Date.now(),
      description: newTaskDescription.value,
      completed: false,
    });
    newTaskDescription.value = ""; // Limpiar el input
  }
};
</script>
