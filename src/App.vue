<template>
  <div class="container">
    <Header title="Task Tracker" />
    <AddTask @add-task="addTask" />
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
        tasks: []
      }
    },
    methods: {
      deleteTask(id) {
        if (confirm('Are you sure you want to delete this task?')) {
          this.tasks = this.tasks.filter(task => task.id !== id)
        }
      },
      toggleReminder(id) {
        this.tasks = this.tasks.map(task => {
          return task.id === id ? { ...task, reminder: !task.reminder } : task
        })
      },
      addTask(newTask) {
        this.tasks = [...this.tasks, newTask]
      }
    },
    created() {
      this.tasks = [
        {
          id: 1,
          text: 'Study vue.js',
          day: 'June 27th at 4pm',
          reminder: true,
        },
        {
          id: 2,
          text: 'Take a nap',
          day: 'June 27th at 11:30am',
          reminder: true,
        },
        {
          id: 3,
          text: 'Do the laundry',
          day: 'June 27th at 8pm',
          reminder: false,
        },
      ]
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