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
      tasks: [],
      showAddTask: false,
    };
  },
  methods: {
    async handleDeleteTask(id) {
      if (confirm("Are you sure to delete?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: "DELETE",
        });

        let taskIdx = this.tasks.findIndex((task) => task.id == id);

        res.status === 200
          ? this.tasks.splice(taskIdx, 1)
          : alert("Error deleting task");
      }
    },
    async handleToggleReminder(id) {
      let _task = this.tasks.find((task) => task.id == id);
      const updTask = { ..._task, reminder: !_task.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(updTask),
      });

      const data = await res.json();
      
    },
    async handleSaveNewTask(newTask) {
      const res = await fetch("api/tasks", {
        method: "POST",
        headers: {
          "Content-type": "application/json",
        },
        body: JSON.stringify(newTask),
      });
      const data = await res.json();
      console.log(data);
      this.tasks = [...this.tasks, data];
      this.showAddTask = false;
    },
    handleOnChangeShowAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();
      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/${id}`);
      const data = await res.json();
      return data;
    },
  },
  async created() {
    this.tasks = await this.fetchTasks();
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
