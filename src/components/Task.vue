<template>
  <div
    @dblclick="$emit('onToggleReminder', task.id)"
    :class="[task.reminder ? 'reminder' : '', 'task']"
  >
    <h3>
      {{ task.title }}
      <i class="fas fa-times" @click="onDelete(task.id)"></i>
    </h3>
    <p>{{ task.person }}</p>
  </div>
</template>

<script>
export default {
  name: "Task",
  props: {
    task: Object,
  },
  methods: {
    onDelete() {
      this.$emit("onDeleteTask", this.task.id);
    },
    onToggleReminder() {
      this.$emit("onToggleReminder", this.task.id);
    },
  },
};
</script>


<style scope>
.fas {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
</style>
