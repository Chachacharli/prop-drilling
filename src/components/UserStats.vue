<template>
  <div>
    <h3>Task Statistics</h3>
    <p>Total tasks: {{ totalTasks }}</p>
    <p>Completed tasks: {{ completedTasks }}</p>
  </div>
</template>

<script setup>
import { inject, computed } from "vue";

// Inyectar el objeto `user` proporcionado por `App.vue`
const user = inject("user");

// Computed para calcular las estadísticas de tareas
const totalTasks = computed(() => user.value.tasks.length);
const completedTasks = computed(
  () => user.value.tasks.filter((task) => task.completed).length
);
</script>
