<template>
  <div>
    <div v-for="task in tasks" :key="task.id">
        <Task :task="task" @onDeleteTask="onDeleteTask" @onToggleReminder="onToggleReminder" />
    </div>
  </div>
</template>

<script>
import Task from "./Task"
export default {
    components: {
       Task
    },
     props: {
    tasks: Array,
  },
    methods: {
        onDeleteTask(id) {
          this.$emit('onDeleteTask', id)
        },
        onToggleReminder(id) {
          this.$emit('onToggleReminder', id)
        }
    },
};
</script>