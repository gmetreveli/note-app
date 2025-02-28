<template>
  <div class="container">
    <div class="task">
      <!-- title -->
      <div class="title">
        <h1>To Do List</h1>
      </div>
      <!-- form -->
      <div class="form">
        <input 
          type="text" 
          placeholder="New Task" 
          :value="newTask" 
          @input="$emit('update-new-task', $event.target.value)" 
          @keyup.enter="addTask"
        />
        <button @click="addTask"><i class="far fa-plus-square"></i></button>
      </div>
      <!-- task lists -->
      <div class="taskItems">
        <ul>
          <task-item 
          :task="task" 
          v-for="(task, index) in tasks" 
          :key="task.id"
          @remove="removeTask(index)"
          @complete="completeTask(task)"
          ></task-item>
        </ul>
      </div>
      <!-- buttons -->
      <div class="clearBtns">
        <button @click="clearCompleted">Clear completed</button>
        <button @click="clearAll">Clear all</button>
      </div>
      <!-- pending task -->
      <div class="pendingTasks">
        <span>Pending Tasks: {{ incomplete }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import TaskItem from './Task-item.vue';

export default {
  name: 'Task',
  props: {
    tasks: Array,
    incomplete: Number,
    newTask: String, 
  },
  components:{
    TaskItem
  },
  methods: {
    addTask() {
      this.$emit('add-task');
    },
    updateNewTask() {
      this.$emit('update-new-task', this.newTask);
    },
    clearCompleted() {
      this.$emit('clear-completed');
    },
    clearAll() {
      this.$emit('clear-all-tasks');
    },
    removeTask(index){
      this.tasks.splice(index, 1);
    },
    completeTask(task){
      task.completed = !task.completed;
    }
  },
  watch: {
    newTask(newVal) {
      this.updateNewTask();
    },
  },
}
</script>
