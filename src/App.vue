<template>
  <main>
    <!-- Heading -->
    <header>
      <img src="./assets/pinia-logo.svg" alt="pinia logo">
      <h1>Pinia Tasks</h1>
    </header>

    <!-- new task form -->
    <div class="new-task-form">
      <TaskForm />
    </div>

    <!-- filter -->
    <nav class="class">
      <button @click="filter = 'all'">All Tasks</button>
      <button @click="filter = 'favs'">Fav Tasks</button>
    </nav>

    <!-- loading -->
    <div class="loading" v-if="loading">Loading tasks...</div>

    <!-- Task list -->
    <div v-if="filter === 'all'" class="task-list">
      <p>You have {{ totalCount }} tasks left to do</p>
      <div v-for="task in tasks">
        <TaskDetails :task="task"/>
      </div>
    </div>
    <!-- Task list -->
    <div v-if="filter === 'favs'" class="task-list">
      <p>You have {{ favCount }} favorite left to do</p>
      <div v-for="task in favs">
        <TaskDetails :task="task"/>
      </div>
    </div>

<button @click="taskStore.$reset">reset state</button>

  </main>
</template>

<script>

import { ref } from 'vue';
import TaskDetails from './components/TaskDetails.vue';
import TaskForm from './components/TaskForm.vue';
import { useTaskStore } from './stores/TaskStore.js';
import { storeToRefs } from 'pinia';

  export default {
    components: {TaskDetails, TaskForm},
    setup(){
      const taskStore = useTaskStore();

      const { tasks, loading, favs, totalCount, favCount} = storeToRefs(taskStore);

      //fetch
      taskStore.getTasks();

      const filter = ref('all')

      return { taskStore, filter, tasks, loading, favs, totalCount, favCount };
    }
  }
</script>