<template>
  <div v-if="showAddTask">
    <AddTask @add-task="addTask" />
  </div>
  <Tasks
    @toggle-remainder="toggleRemainder"
    @delete-task="deleteTask"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks';
import AddTask from '../components/AddTask';

export default {
  name: 'Home',
  props: {
    showAddTask: Boolean
  },
  components: {
    Tasks,
    AddTask,
  },
  data() {
    return {
      tasks: [],
    }
  },
  methods: {
    async addTask (task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(task)
      })
      const data = await res.json();
      this.tasks = [...this.tasks, data]
    },
    async deleteTask (id) {
      if(confirm("Are you sure?")){
        const response = await fetch(`api/tasks/${id}`,{
          method: 'DELETE'
        })
        response.status === 200 ? (
          this.tasks = this.tasks.filter(t => t.id !== id)
        ) : alert("Error Deleting task")
      }
    },
    async toggleRemainder (id) {
      const taskToToggle = await this.fetchTask(id)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder}

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json()
      this.tasks = this.tasks.map(t => t.id === id ? {...t, reminder: data.reminder} : t)
    },
    async fetchTasks() {
      const response = await fetch('api/tasks')
      const data = await response.json()
      return data
    },
    async fetchTask(id) {
      const response = await fetch(`api/tasks/${id}`)
      const data = await response.json()
      return data
    }
  },
  async created() {
    this.tasks = await this.fetchTasks()
  }
}
</script>
