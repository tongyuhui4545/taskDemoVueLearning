<template>
  <header>
    <h1>{{ title }}</h1>
    <Button
      :text="buttonText"
      :color="buttonColor"
      @onChangeShowAddTask="onChangeShowAddTask"
    />
  </header>
</template>

<script>
import Button from "./Button";

export default {
  props: ["title", "showAddTask"],
  components: {
    Button,
  },
  methods: {
    onChangeShowAddTask() {
      this.$emit("onChangeShowAddTask");
    },
  },
  computed: {
    buttonText() {
      if (this.showAddTask) {
        return "Close";
      } else return "Add Task";
    },
    buttonColor() {
      if (this.showAddTask) {
        return "red";
      } else return "green";
    },
  },
};
</script>

<style scoped>
header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>