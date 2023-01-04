<template>
  <div v-for="task in tasks" :key="task.id">
    <Task @toggle-remainder="$emit('toggle-remainder', task.id)" @delete-task="$emit('delete-task', task.id)" :task="task" />
  </div>
</template>

<script>
import Task from './Task';

export default {
  name: 'Tasks',
  components: { Task },
  props: {
    tasks: Array
  },
  emits: ['delete-task', 'toggle-remainder']
}
</script>
