<template>
  <div class="container">
    <Header 
      title="Task Tracker" 
      @toggle-add-task="toggleAddTask" 
      v-bind:showAddTask="showAddTask"
    />
    <div v-show="showAddTask">
      <AddTask @add-task="addTask" />
    </div>
    <Tasks 
      v-bind:tasks="tasks" 
      @delete-task="deleteTask" 
      @toggle-reminder="toggleReminder"
    />
  </div>
</template>

<script>
  import Header from './components/Header'
  import Tasks from './components/Tasks'
  import AddTask from './components/AddTask'

  export default {
    name: 'App',
    components: {
      Header,
      Tasks,
      AddTask
    },
    data() {
      return {
        tasks: [],
        showAddTask: false,
      }
    },
    methods: {
      async deleteTask(id) {
        if (confirm('Are you sure you want to delete this task?')) {
          const res = await fetch(`api/tasks/${id}`, {
            method: 'DELETE'
          })

          if (res.status === 200) {
            this.tasks = this.tasks.filter(task => task.id !== id)
          } else {
            alert('Sorry, something went wrong :(')
          }

        }
      },
      async toggleReminder(id) {
        const taskToToggle = await this.fetchTask(id)
        const updatedTask = {...taskToToggle, reminder: !taskToToggle.reminder}

        const res = await fetch(`api/tasks/${id}`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(updatedTask)
        })

        if (res.status === 200) {
          this.tasks = this.tasks.map(task => {
            return task.id === id ? { ...task, reminder: updatedTask.reminder } : task
          })
        } else {
            alert('Sorry, something went wrong :(')
        }
      },
      async addTask(newTask) {
        const res = await fetch('api/tasks', {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json'
          },
          body: JSON.stringify(newTask)
        })
        const data = await res.json()

        this.tasks = [...this.tasks, data] 
      },
      toggleAddTask() {
        this.showAddTask = !this.showAddTask
      },
      async fetchTasks() {
        const res = await fetch('api/tasks')
        const data = res.json()
        return data
      },
      async fetchTask(id) {
        const res = await fetch(`api/tasks/${id}`)
        const data = res.json()
        return data
      }
    },
    async created() {
      this.tasks = await this.fetchTasks()
    }
  }
</script>

<style>
  @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap');

  * {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
  }

  body {
    font-family: 'Poppins', sans-serif;
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