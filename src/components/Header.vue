<template>
  <header>
    <h1>Task Tracker</h1>
    <Button
      @btn-click="$emit('toggle-add-task')"
      :text="showAddTask ? 'Close' : 'Add Task'"
      :color="showAddTask ? 'red' : 'green'" />
  </header>
</template>


<script>
import Button from './Button.vue'

export default {
  name: 'Header',
  props: {
    showAddTask: Boolean
  },
  components: {
    Button
  },
  emits: ['btn-click']
}
</script>

<style scoped>
header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
}
</style>
