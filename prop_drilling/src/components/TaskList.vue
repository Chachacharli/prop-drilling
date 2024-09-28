<template>
  <div>
    <h3>Task List</h3>
    <ul>
      <li v-for="task in user.tasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" />
        <span :class="{ completed: task.completed }">{{
          task.description
        }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { inject } from "vue";

// Inyectar el objeto `user`
const user = inject("user");
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>
