<template>
  <div class="container">
    <div class="todo-app">
      <h2>Schedule Things<i class="fa-solid fa-list"></i></h2>
      <div class="row">
        <input v-model="newTask" @keyup.enter="addTask" type="text" placeholder="Add your text" />
        <button @click="addTask">Add Task</button>
      </div>
      <ul>
        <li
          v-for="(task, index) in tasks"
          :key="index"
          :class="{ checked: task.completed }"
          @click="toggleCheck(task)"
        >
          <span 
          @click="toggleTask(index)" class="checkbox">
        </span>
          {{ task.text }}
          <span @click="removeTask(index)" class="delete">×</span>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tasks: [],
      newTask: "",
      completed: false,
      chec: false
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim() !== "") {
        this.tasks.push({ text: this.newTask, completed: false });
        // this.tasks.classList("checked");
        this.newTask = "";
        this.saveData();
      } else {
        alert("Field must be filled");
      }
    },
    toggleCheck(task) {
      task.completed = !task.completed;
    },
    toggleTask(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
      this.saveData();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveData();
    },
    saveData() {
      localStorage.setItem("tasks", JSON.stringify(this.tasks));
    },
    loadData() {
      const savedTasks = localStorage.getItem("tasks");
      this.tasks = savedTasks ? JSON.parse(savedTasks) : [];
    },
  },
  created() {
    this.loadData();
  },
};
</script>
