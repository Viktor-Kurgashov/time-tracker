<template>
  <div class="completed">
    <span class="completed-title">{{ title }}</span>

    <span v-if="desc" class="completed-desc">{{ desc }}</span>

    <div class="completed-checkmark">
      <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 512 512">
        <path fill="currentColor" d="M173.898 439.404l-166.4-166.4c-9.997-9.997-9.997-26.206 0-36.204l36.203-36.204c9.997-9.998 26.207-9.998 36.204 0L192 312.69 432.095 72.596c9.997-9.997 26.207-9.997 36.204 0l36.203 36.204c9.997 9.997 9.997 26.206 0 36.204l-294.4 294.401c-9.998 9.997-26.207 9.997-36.204-.001z"/>
      </svg>
    </div>

    <span class="completed-timer">{{ time }}</span>

    <button class="completed-delete-btn" @click="$emit('taskDeleted', this.id)">
      <svg xmlns="http://www.w3.org/2000/svg" aria-hidden="true" focusable="false" role="img" viewBox="0 0 352 512">
        <path fill="currentColor" d="M242.72 256l100.07-100.07c12.28-12.28 12.28-32.19 0-44.48l-22.24-22.24c-12.28-12.28-32.19-12.28-44.48 0L176 189.28 75.93 89.21c-12.28-12.28-32.19-12.28-44.48 0L9.21 111.45c-12.28 12.28-12.28 32.19 0 44.48L109.28 256 9.21 356.07c-12.28 12.28-12.28 32.19 0 44.48l22.24 22.24c12.28 12.28 32.2 12.28 44.48 0L176 322.72l100.07 100.07c12.28 12.28 32.2 12.28 44.48 0l22.24-22.24c12.28-12.28 12.28-32.19 0-44.48L242.72 256z"/>
      </svg>
    </button>

  </div>
</template>



<script>
export default {
  name: 'TaskCompleted',

  props: ['id', 'title', 'desc', 'time']
}
</script>



<style>
  .completed {
    background-color: #f8f8f8;
    border-radius: 10px;
    margin-top: 25px;
    padding: 15px;

    display: grid;
    grid-template:
      "title title title" auto
      "desc desc desc" auto
      "checkmark timer delete-btn" 57px
      / auto 1fr auto;
    align-items: end;
  }
  .completed > * {
    height: 45px;
  }


  .completed-title,
  .completed-desc {
    padding: 0 15px;
    line-height: 44px;
    border-bottom: 1px solid #dfdfdf;
  }
  .completed-title {
    grid-area: title;
    font-size: 1.5rem;
  }
  .completed-desc {
    grid-area: desc;
    margin-top: 8px;
    font-size: 1.3rem;
    color: #595959;
  }


  .completed-checkmark,
  .completed-delete-btn {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 45px;
  }



  .completed-checkmark {
    grid-area: checkmark;
  }
  .completed-checkmark > svg {
    color: #4d4d4d;
    height: 30px;
  }


  .completed-timer {
    grid-area: timer;
    justify-self: center;
    line-height: 45px;
    font-size: 1.75rem;
  }


  .completed-delete-btn {
    grid-area: delete-btn;
    background: transparent;
    cursor: pointer;
    border: none;
  }
  .completed-delete-btn > svg {
    color: #4d4d4d;
    height: 33px;
  }
  .completed-delete-btn:active > svg {
    color: #d3d3d3;
  }
</style>