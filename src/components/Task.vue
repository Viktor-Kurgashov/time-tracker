<template>
  <div class="task">

    <!-- НАЗВАНИЕ ЗАДАЧИ -->
    <div class="task-title-wrapper text-input">
      <span class="task-title-placeholder text-input__placeholder">
        Задача
      </span>
  
      <input class="task-title text-input__field"
        type="text"
        v-model="title"
        @focus="removePlaceholder"
        @blur="addPlaceholder">
    </div>


    <!-- КНОПКА ОТКРЫТЬ ОПИСАНИЕ -->
    <button class="btn task-desc-open-btn" @click="descOpened = !descOpened">
      <svg v-if="descOpened" class="btn-icon"      xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 448 512">
        <path fill="currentColor" d="M240.971 130.524l194.343 194.343c9.373 9.373 9.373 24.569 0 33.941l-22.667 22.667c-9.357 9.357-24.522 9.375-33.901.04L224 227.495 69.255 381.516c-9.379 9.335-24.544 9.317-33.901-.04l-22.667-22.667c-9.373-9.373-9.373-24.569 0-33.941L207.03 130.525c9.372-9.373 24.568-9.373 33.941-.001z"/>
      </svg>
  
      <svg v-else class="btn-icon"      xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 448 512">
        <path fill="currentColor" d="M207.029 381.476L12.686 187.132c-9.373-9.373-9.373-24.569 0-33.941l22.667-22.667c9.357-9.357 24.522-9.375 33.901-.04L224 284.505l154.745-154.021c9.379-9.335 24.544-9.317 33.901.04l22.667 22.667c9.373 9.373 9.373 24.569 0 33.941L240.971 381.476c-9.373 9.372-24.569 9.372-33.942 0z"/>
      </svg>
    </button>
  

    <!-- ОПИСАНИЕ ЗАДАЧИ -->
    <div v-show="descOpened" class="task-desc-wrapper text-input">
      <span class="task-desc-placeholder text-input__placeholder">
        Описание
      </span>

      <input class="task-desc text-input__field"
        type="text"
        v-model="desc"
        @focus="removePlaceholder"
        @blur="addPlaceholder">
    </div>


    <!-- СТАРТ СТОП ЗАДАЧИ -->
    <button class="btn task-play-btn" @click="timerRunning = !timerRunning">
      <svg v-if="timerRunning" class="btn-icon"      xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 448 512">
        <path fill="currentColor" d="M144 479H48c-26.5 0-48-21.5-48-48V79c0-26.5 21.5-48 48-48h96c26.5 0 48 21.5 48 48v352c0 26.5-21.5 48-48 48zm304-48V79c0-26.5-21.5-48-48-48h-96c-26.5 0-48 21.5-48 48v352c0 26.5 21.5 48 48 48h96c26.5 0 48-21.5 48-48z"/>
      </svg>
  
      <svg v-else class="btn-icon"      xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 448 512">
        <path fill="currentColor" d="M424.4 214.7L72.4 6.6C43.8-10.3 0 6.1 0 47.9V464c0 37.5 40.7 60.1 72.4 41.3l352-208c31.4-18.5 31.5-64.1 0-82.6z"/>
      </svg>
    </button>


    <!-- ВРЕМЯ ЗАДАЧИ -->
    <span class="task-timer">
      {{ timerString }}
    </span>


    <!-- КНОПКА ЗАВЕРШИТЬ -->
    <button class="btn task-complete-btn" @click="emitTaskCompleted">
      <svg class="btn-icon" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 512 512">
        <path fill="currentColor" d="M173.898 439.404l-166.4-166.4c-9.997-9.997-9.997-26.206 0-36.204l36.203-36.204c9.997-9.998 26.207-9.998 36.204 0L192 312.69 432.095 72.596c9.997-9.997 26.207-9.997 36.204 0l36.203 36.204c9.997 9.997 9.997 26.206 0 36.204l-294.4 294.401c-9.998 9.997-26.207 9.997-36.204-.001z"/>
      </svg>
    </button>


    <!-- КНОПКА УДАЛИТЬ -->
    <button class="btn task-delete-btn" @click="$emit('taskDeleted', this.id)">
      <svg class="btn-icon" xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 352 512">
        <path fill="currentColor" d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"/>
      </svg>
    </button>

  </div>
</template>



<script>
export default {
  name: 'Task',

  props: [ 'id' ],

  data () {
    return {
      descOpened: true,

      timerRunning: false,
      timerCount: 0,
      timerId: undefined,

      title: '',
      desc: ''
    }
  },


  computed: {
    timerString () {
      return (Math.floor(this.timerCount / 60) + '').padStart(2, 0) + ':' + (this.timerCount % 60 + '').padStart(2, 0)
    }
  },


  watch: {
    timerRunning (newValue) {
      if (newValue) this.timerId = setInterval(() => this.timerCount += 1, 1000)
      else clearInterval(this.timerId)
    }
  },


  methods: {
    removePlaceholder (event) {
      event.target.previousElementSibling.hidden = true;
    },
    addPlaceholder (event) {
      if (!event.target.value) event.target.previousElementSibling.hidden = false;
    },


    emitTaskCompleted () {      
      this.timerRunning = false;

      this.$emit('taskCompleted', {
        id: this.id,
        title: this.title,
        desc: this.desc,
        time: this.timerString
      })
    }
  }
}
</script>



<style>
  .task {
    background-color: #f8f8f8;
    border-radius: 10px;
    margin-top: 25px;
    padding: 15px;
    display: grid;
    grid-template:
      "title title title desc-btn" auto
      "desc desc desc desc" auto
      "play-btn timer complete-btn delete-btn" 57px
      / auto 1fr auto auto;
    align-items: end;
  }
  .btn {
    font-size: 0;
    width: 45px;
    height: 45px;
    padding: 0;
    border: none;
    background: transparent;
    cursor: pointer;
  }
  .btn-icon {
    color: #4d4d4d;
    height: 30px;
  }



  .text-input,
  .text-input__field {
    position: relative;
  }
  .text-input__field,
  .text-input__placeholder {
    width: 100%;
    height: 100%;
    padding: 0 15px;
  }
  .text-input__field {
    z-index: 1;
    outline: none;
    border: none;
    border-bottom: 1px solid #dfdfdf;
    background-color: transparent;
    color: #404040;
  }
  .text-input__field:focus {
    border-bottom-color: #595959;
  }
  .text-input__placeholder {
    position: absolute;
    color: #d3d3d3;
  }



  .task-title-wrapper,
  .task-desc-wrapper {
    height: 45px;
  }
  .task-title-wrapper {
    grid-area: title;
  }
  .task-desc-wrapper {
    grid-area: desc;
    margin-top: 8px;
  }
  .task-title-placeholder,
  .task-desc-placeholder {
    font-size: 1.5rem;
    line-height: 44px;
  }
  .task-title {
    font-size: 1.5rem;
  }
  .task-desc {
    font-size: 1.3rem;
    color: #595959;
  }



  .task-desc-open-btn {
    grid-area: desc-btn;
    margin-left: 5px;
  }
  .task-desc-open-btn > svg:first-child {
    margin-top: 4px;
  }
  .task-desc-open-btn > svg:last-child {
    margin-left: 3px;
  }


  
  .task-timer {
    grid-area: timer;
    justify-self: center;
    height: 45px;
    line-height: 45px;
    font-size: 1.75rem;
  }



  .task-play-btn {
    grid-area: play-btn;
  }
  .task-play-btn > svg:first-child {
    height: 27px;
    margin-left: 3px;
  }
  .task-complete-btn {
    grid-area: complete-btn;
    margin-right: 7px;
  }
  .task-delete-btn {
    grid-area: delete-btn;
    margin-left: 5px;
  }
  .task-delete-btn > svg {
    height: 33px;
  }
  .task-complete-btn:active > svg,
  .task-delete-btn:active > svg {
    color: #d3d3d3;
  }
</style>