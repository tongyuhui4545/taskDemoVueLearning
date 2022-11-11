<template>
  <div class="container">
    <Header
      title="Task Tracker"
      @onChangeShowAddTask="handleOnChangeShowAddTask"
      :showAddTask="showAddTask"
    ></Header>
    <div v-if="showAddTask">
      <AddTask @onSaveNewTask="handleSaveNewTask" />
    </div>
    <Tasks
      :tasks="tasks"
      @onDeleteTask="handleDeleteTask"
      @onToggleReminder="handleToggleReminder"
    />
  </div>
</template>

<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

export default {
  name: "App",
  components: {
    Header,
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [
        { id: 1, title: "eat rice", person: "John", reminder: false },
        { id: 2, title: "drink coffee", person: "Miller", reminder: false },
        { id: 3, title: "go hiking", person: "Bob", reminder: false },
      ],
      showAddTask: false,
    };
  },
  methods: {
    handleDeleteTask(id) {
      if (confirm("Are you sure to delete?")) {
        let taskIdx = this.tasks.findIndex((task) => task.id == id);
        this.tasks.splice(taskIdx, 1);
      }
    },
    handleToggleReminder(id) {
      let _task = this.tasks.find((task) => task.id == id);
      _task.reminder = !_task.reminder;
    },
    handleSaveNewTask(newTask) {
      this.tasks.push(newTask);
      this.showAddTask = false;
    },
    handleOnChangeShowAddTask() {
      this.showAddTask = !this.showAddTask;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
body {
  font-family: "Poppins", sans-serif;
}
.container {
  max-width: 500px;
  margin: 30px auto;
  overflow: auto;
  min-height: 300px;
  border: 1px solid steelblue;
  padding: 30px;
  border-radius: 5px;
}
.btn {
  display: inline-block;
  background: #000;
  color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 5px;
  border-radius: 5px;
  cursor: pointer;
  text-decoration: none;
  font-size: 15px;
  font-family: inherit;
}
.btn:focus {
  outline: none;
}
.btn:active {
  transform: scale(0.98);
}
.btn-block {
  display: block;
  width: 100%;
}
</style>
