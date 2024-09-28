---
tittle: "Prop drilling"
---

# Prop Drilling

## ¿Qué es el prop drilling?

 ES un problema común en el desarrollo de aplicaciones con componentes anidados, en donde los props deben de pasar de un componente a otro, aunque no sean necesarios en los componentes intermedios.

 ## Ejemplo

 ```
// App.vue
<template>
  <Parent :message="message" />
</template>

<script setup>
const message = "Hello from App!";
</script>

// Parent.vue
<template>
  <Child :message="message" />
</template>

<script setup>
defineProps(['message']);
</script>

// Child.vue
<template>
  <GrandChild :message="message" />
</template>

<script setup>
defineProps(['message']);
</script>

// GrandChild.vue
<template>
  <div>{{ message }}</div>
</template>

<script setup>
defineProps(['message']);
</script>

 ```

Aquí, el prop message se define en App.vue y debe ser pasado manualmente a través de Parent y Child, incluso si esos componentes no lo usan. Esto genera varios problemas conforme la aplicación crece:

Complejidad: A medida que el árbol de componentes se hace más profundo, se vuelve tedioso y propenso a errores pasar las propiedades manualmente.
Legibilidad: Los componentes intermedios (Parent, Child) tienen que manejar props que no les conciernen directamente, lo cual afecta la claridad del código.
Mantenimiento: Si hay cambios en la propiedad (nombre o estructura), debes actualizar todos los niveles por los que pasa.
Event Frothing: Es el mismo caso que el prop drilling, pero a la inversa, si queremos pasar este componente actualizado será un problema.

## Soluciones
### Provide/Inject

#### Ejemplo
Tendrás un objeto user que incluye datos del usuario y una lista de tareas. Los componentes intermedios no manejan las tareas, pero los componentes más bajos podrán agregar, editar, y marcar tareas como completadas. Habrá varios componentes que interactúan con diferentes partes del objeto user, lo que implica que cada componente tiene una responsabilidad diferente (crear tareas, listar tareas, mostrar estadísticas, etc.).

```
App
 ├── UserPanel
 |    └── UserStats (Muestra estadísticas del usuario)
 └── TaskManager
      ├── TaskList (Muestra las tareas)
      └── TaskForm (Permite agregar nuevas tareas)
```


```js
// App.vue
<template>
  <div>
    <h1>{{ user.name }}'s Task Manager</h1>
    <UserPanel />
    <TaskManager />
  </div>
</template>

<script setup>
import { ref, provide } from 'vue';
import UserPanel from './components/UserPanel.vue';
import TaskManager from './components/TaskManager.vue';

// Objeto reactivo del usuario con una lista de tareas
const user = ref({
  name: 'Alice',
  tasks: [
    { id: 1, description: 'Learn Vue 3', completed: false },
    { id: 2, description: 'Build a cool app', completed: false }
  ]
});

// Proveer el objeto `user` a los componentes descendientes
provide('user', user);
</script>

```

```js
// UserPanel.vue
<template>
  <div class="user-panel">
    <h2>User Panel</h2>
    <UserStats />
  </div>
</template>

<script setup>
import UserStats from './UserStats.vue';
</script>
```

```js
// UserStats.vue
<template>
  <div>
    <h3>Task Statistics</h3>
    <p>Total tasks: {{ totalTasks }}</p>
    <p>Completed tasks: {{ completedTasks }}</p>
  </div>
</template>

<script setup>
import { inject, computed } from 'vue';

// Inyectar el objeto `user` proporcionado por `App.vue`
const user = inject('user');

// Computed para calcular las estadísticas de tareas
const totalTasks = computed(() => user.value.tasks.length);
const completedTasks = computed(() => user.value.tasks.filter(task => task.completed).length);
</script>
```

```js	

// TaskManager.vue
<template>
  <div class="task-manager">
    <h2>Manage Tasks</h2>
    <TaskForm />
    <TaskList />
  </div>
</template>

<script setup>
import TaskForm from './TaskForm.vue';
import TaskList from './TaskList.vue';
</script>

```

```js
<template>
  <div>
    <h3>Add New Task</h3>
    <input v-model="newTaskDescription" placeholder="New task" />
    <button @click="addTask">Add Task</button>
  </div>
</template>

<script setup>
import { inject, ref } from 'vue';

// Inyectar el objeto `user`
const user = inject('user');

// Variable para almacenar la descripción de la nueva tarea
const newTaskDescription = ref('');

// Función para agregar una nueva tarea al objeto `user`
const addTask = () => {
  if (newTaskDescription.value.trim() !== '') {
    user.value.tasks.push({
      id: Date.now(),
      description: newTaskDescription.value,
      completed: false
    });
    newTaskDescription.value = ''; // Limpiar el input
  }
};
</script>
```

```js	
// TaskList.vue
<template>
  <div>
    <h3>Task List</h3>
    <ul>
      <li v-for="task in user.tasks" :key="task.id">
        <input type="checkbox" v-model="task.completed" /> 
        <span :class="{ completed: task.completed }">{{ task.description }}</span>
      </li>
    </ul>
  </div>
</template>

<script setup>
import { inject } from 'vue';

// Inyectar el objeto `user`
const user = inject('user');
</script>

<style scoped>
.completed {
  text-decoration: line-through;
  color: gray;
}
</style>

```
#### Se debe de: 
Extender la funcionalidad:
  1. Agregar un botón para eliminar tareas desde TaskList.vue.
  2. Crear un nuevo componente que muestre tareas específicas (e.g., solo las completadas o pendientes).

Personalizar: 
Se debe de cambiar el diseño o la estructura de los datos (e.g., agregar una fecha límite a las tareas, priorizar las tareas, etc.), pero manteniendo la lógica de provide/inject.
