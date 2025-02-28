<template>
  <div id="app">
    <task 
      :tasks="tasks" 
      :incomplete="incomplete" 
      :newTask="newTask" 
      @clear-all-tasks="clearTasks" 
      @clear-completed="clearCompleted" 
      @add-task="addTask" 
      @update-new-task="updateNewTask"
    ></task>
  </div>
</template>

<script lang="ts">
import Task from './components/Task.vue'

export default {
  name: 'App',
  components: {
    Task,
  },
  data() {
    return {
      tasks: [
        { id: 1, title: 'Learn Vue JS', completed: true },
        { id: 2, title: 'Watch netflix', completed: true },
        { id: 3, title: 'Go shopping', completed: false },
        { id: 4, title: 'Learn guitar', completed: false },
        { id: 5, title: 'Send email', completed: false },
      ],
      newTask: "",
    }
  },
  computed: {
    incomplete() {
      return this.tasks.filter(this.inProgress).length;
    },
  },
  methods: {
    addTask() {
      if (this.newTask) {
        this.tasks.push({
          title: this.newTask,
          completed: false,
        });
        this.newTask = "";
      }
    },
    updateNewTask(newTask) {
      this.newTask = newTask;
    },
    inProgress(task) {
      return !this.isCompleted(task);
    },
    isCompleted(task) {
      return task.completed;
    },
    clearCompleted() {
      this.tasks = this.tasks.filter(this.inProgress);
    },
    clearTasks() {
      this.tasks = [];
    },
  },
}
</script>
