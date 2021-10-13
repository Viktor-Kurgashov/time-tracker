<template>
  <div class="time-tracker">

    <div class="switch-tab">
      <button class="create-tab-btn" @click="createTask">
        <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 448 512">
          <path fill="currentColor" d="M416 208H272V64c0-17.67-14.33-32-32-32h-32c-17.67 0-32 14.33-32 32v144H32c-17.67 0-32 14.33-32 32v32c0 17.67 14.33 32 32 32h144v144c0 17.67 14.33 32 32 32h32c17.67 0 32-14.33 32-32V304h144c17.67 0 32-14.33 32-32v-32c0-17.67-14.33-32-32-32z"/>
        </svg>
      </button>

      <button class="active-tab-btn" :class="{ selectedd: activeTabSelected }" @click="activeTabSelected = true">Текущие</button>

      <button class="completed-tab-btn" :class="{ selectedd: !activeTabSelected }" @click="activeTabSelected = false">Выполненные</button>
    </div>  


    <div v-show="activeTabSelected" class="active-tasks">
      <Task
        v-for="task of tasks.filter(item => item.active)"
        :key="task.id"
        :id="task.id"
        @task-completed="completeTask"
        @taskDeleted="deleteTask"
      />
    </div>


    <div v-show="activeTabSelected == false" class="completed-tasks">
      <TaskCompleted
        v-for="task of tasks.filter(item => !item.active)"
        :key="task.id"
        :id="task.id"
        :title="task.title"
        :desc="task.desc"
        :time="task.time"
        @taskDeleted="deleteTask"
      />
    </div>

  </div>
</template>



<script>
import Task from './Task.vue'
import TaskCompleted from './TaskCompleted.vue'


export default {
  name: 'TimeTracker',

  components: { Task, TaskCompleted },

  data () {
    return {
      lastId: 0,
      activeTabSelected: true,
      tasks: [ /* { active: true, id: '', title: '', desc: '', time: '' } */  ]
    }
  },


  methods: {
    createTask () {
      this.activeTabSelected = true;

      this.tasks.unshift({
        active: true,
        id: 'task' + (this.lastId += 1)
      })
    },

    completeTask (value) {
      const item = this.tasks[this.tasks.findIndex(item => item.id === value.id)];

      item.active = false;
      item.title = value.title;
      item.desc = value.desc;
      item.time = value.time;
    },

    deleteTask (value) {
      this.tasks.splice(this.tasks[this.tasks.findIndex(item => item.id === value)], 1)
    }
  }
}
</script>



<style>
  .time-tracker {
    max-width: 500px;
    margin: 0 auto;
  }
  .switch-tab {
    display: flex;
    justify-content: center;
    height: 45px;
  }


  .create-tab-btn,
  .active-tab-btn,
  .completed-tab-btn {
    border: 1px solid #dfdfdf;
    background: #f8f8f8;
    color: #404040;
    cursor: pointer;
    font-size: 1.4rem;
    padding: 0 10px;
  }
  .selectedd {
    background: #dfdfdf;
  }


  .create-tab-btn {
    border-radius: 5px 0 0 5px;
    border-right: none;
    padding: 0 15px;
    font-size: 0;
  }
  .create-tab-btn > svg {
    color: #4d4d4d;
    height: 24px;
  }
  .create-tab-btn:active > svg {
    color: #d3d3d3;
  }


  .completed-tab-btn {
    border-radius: 0 5px 5px 0;
    border-left: none;
  }
</style>