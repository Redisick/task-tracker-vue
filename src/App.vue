<template>
  <Header 
  @toggle-add-task="toggleAddTask"
  title='Диспетчер задач'
  :showAddTask="showAddTask"
  />
  <div class="form--container" v-if="showAddTask">
    <AddTask  @toggle-add-task="toggleAddTask"
    @add-task="addTask" :task="task"/>
  </div>

  <Filter @filter-change="filter"/>

  <Tasks 
  @update-task="updateTask"
  @toggle-done="toggleDone"
  @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>
import Header from './components/Header';
import Tasks from './components/Tasks';
import AddTask from './components/AddTask';
import Filter from './components/Filter';

export default{
  name: "App",
  components: {
    Header,
    AddTask,
    Tasks,
    Filter
  },
  data() {
    return {
      tasks: [],
      tasksBackup: [],
      showAddTask: false,
      task: {}
    }
  },
  methods: {
    async addTask(task, id) {

      //update
      if (id){
        const taskToUpdate = await this.fetchTask(id);
        const updTask = {...taskToUpdate, text: task.text};

        const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
        })

        const data = await res.json();

        this.tasks = this.tasks.map((task) => 
        task.id === id ? {...task, text: data.text} : task)
      }

      //add new
      else {
        const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(task)
      })

      const data = await res.json();

      this.tasks = [...this.tasks, data];
      }

      //reset
      this.showAddTask = false;
      this.task = {};   
    },
    async deleteTask(id){
      if (confirm('Уверены, что хотите удалить задачу?')){
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE'
        });
        res.status === 200 ? 
        (this.tasks = this.tasks.filter((task) => task.id !== id)) :
        alert('Ошибка при удалении задачи');
      }  
    },
    async toggleDone(id){
      const taskToToggle = await this.fetchTask(id);
      const updTask = {...taskToToggle, done: !taskToToggle.done};

      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(updTask)
      })

      const data = await res.json();

      this.tasks = this.tasks.map((task) => 
      task.id === id ? {...task, done: data.done} : task)
    },
    async updateTask(id){   
      const taskToUpdate = await this.fetchTask(id);
      this.task = taskToUpdate;
      this.showAddTask = true;
    },
    toggleAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = await res.json();

      return data;
    },
    async fetchTasksFiltered(filtering) {
      const res = await fetch('api/tasks');
      let data = await res.json();

      if (filtering === 'checked'){
        data = data.filter((task) => task.done);
      }
      else if (filtering === 'unchecked'){ 
        data = data.filter((task) => !task.done);
      }

      return data;
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();

      return data;
    },
    async filter(filtering) {
      console.log(filtering);
      this.tasks = await this.fetchTasksFiltered(filtering);      
    }
  },
  async created() {
    this.tasks = await this.fetchTasks();
  }
};
</script>
