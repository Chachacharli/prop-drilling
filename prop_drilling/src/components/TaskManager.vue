<template>
  <div class="task-manager">
    <h2>Manage Tasks</h2>
    <TaskForm />
    <TaskList />
  </div>
</template>

<script setup>
import TaskForm from "./TaskForm.vue";
import TaskList from "./TaskList.vue";
</script>
