<template>
  <div class="new-task">
    <button class="new-task__open new-task__button" type="button"
      v-if="!isOpened" @click="isOpened=!isOpened">Новая задача</button>
    <form class="new-task__fields"
      v-if="isOpened" @submit.prevent="onSubmit">
      <label class="new-task__wrapper">
        <span class="new-task__value">Заголовок</span>
        <input class="new-task__text new-task__border-style" type="text" placeholder="Прочитать книгу"
          v-model="title">
      </label>
      <label class="new-task__wrapper">
        <span class="new-task__value">Заметки</span>
        <textarea class="new-task__text new-task__text--area new-task__border-style" placeholder="Я сделаю это за 5 дней"
          v-model="description"></textarea>
      </label>
      <div class="new-task__wrapper">
        <span class="new-task__value">Категория</span>
        <select class="new-task__category new-task__border-style"
          v-model="category">
          <option value disabled selected>Выберете категорию</option>
          <option v-for="item of categories" :value="item">{{item}}</option>
        </select>
      </div>
      <div class="new-task__buttons">
        <button class="new-task__submit new-task__button" type="submit" v-bind:disabled="isDisabled">Создать</button>
        <button class="new-task__cancel new-task__button" @click="isOpened = !isOpened">Отмена</button>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  props: ["categories"],
  data() {
    return {
      isOpened: false,
      title: '',
      description: '',
      category: ''
    }
  },
  computed: {
    isDisabled() {
      return !(this.title && this.description && this.category);
    }
  },
  methods: {
    onSubmit() {
      if (this.title.trim()) {
        const newTask = {
          id: Date.now(),
          title: this.title,
          category: this.category,
          description: this.description,
          date: new Date(),
          progress: 0
        }
        this.$emit('add-task', newTask);
      }
      this.title = "";
      this.description = "";
      this.category = "";
      this.isOpened = false;
    }
  }
}
</script>

<style lang="less">
  .new-task__fields {
    display: flex;
    flex-direction: column;
    padding-left: 15px;
    padding-right: 15px;
    box-sizing: border-box;
    width: 100%;
  }

  .new-task__value {
    display: block;
    margin: 10px 0;

    text-align: left;
    font-size: 17px;
    font-weight: 700;
  }

  .new-task__text {
    display: block;
    box-sizing: border-box;
    width: 100%;
    margin: 0;
    padding: 8px 10px;

    font-size: 17px;

    cursor: text;
  }

  .new-task__text--area {
    max-height: 600px;
    min-height: 120px;
  }

  .new-task__category {
    display: block;
    padding: 5px;
    font-size: 17px; 
  }

  .new-task__border-style {
    box-shadow: inset 0 1px 2px rgba(10,10,10,.1);
    border: 1px solid transparent;
    border-color: #DAD8D9;
    border-radius: 4px;
  }

  .new-task__buttons {
    display: flex;
    margin-top: 20px;
  }

  .new-task__button {
    display: block;

    cursor: pointer;

    font-size: 17px;

    background: none;
    border: none;
  }

  .new-task__open {
    display: block;
    width: 100%;
    padding: 10px;

    color: white;

    background-color: #052749;
    border-color: transparent;
    border-radius: 4px;
    
  }

  .new-task__submit {
    padding: 5px 12px;
    margin-right: 20px;

    color: white;

    background-color: #052749;
    border-color: transparent;
    border-radius: 4px;

    &:disabled {
      cursor: default;
      opacity: 0.5;
    }
  }

  .new-task__cancel {
    padding: 0;

    color: inherit;

    text-decoration: underline;
  }
</style>
