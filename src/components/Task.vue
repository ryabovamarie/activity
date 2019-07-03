<template>
  <li class="task">
    <div class="task__wrapper">
      <div class="task__wrapper--top">
        <h2 class="task__title" v-if="!isEdit">{{item.title}}</h2>
        <input class="task__text" type="text" v-if="isEdit" v-model="title">

        <p class="task__category">{{item.category}}</p>

        <p class="task__description" v-if="!isEdit">{{item.description}}</p>
        <textarea class="task__text task__text--area" v-if="isEdit" v-model="description"></textarea>
      </div>
      
      <div class="task__buttons" v-if="!isEdit">
        <button class="task__edit" @click="isEdit=!isEdit">
          <img width="25px" height="25px" src="@/assets/settings-work-tool.svg" alt="редактировать">
        </button>
        <button class="task__delete" @click="$emit('remove-task', item.id)">
          <img width="25px" height="25px" src="@/assets/waste-bin.svg" alt="удалить">
        </button>
      </div>
    </div>

    <div class="task__wrapper task__wrapper--bottom">
      <p class="task__date">{{item.date | toString}}</p>
      <input class="task__range" type="range" min="0" max="100" step="10" v-if="isEdit" v-model="progress">
      <p class="task__progress">Прогресс: <span v-bind:style="{ color: fontColor }">{{progress}}%</span></p>
    </div>

    <div class="task__edit-buttons" v-if="isEdit">
      <button class="task__update" @click="updateTask">Обновить</button>
      <button class="task__cancel" @click="cancel">Отмена</button>
    </div>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      isEdit: false,
      title: this.item.title,
      description: this.item.description,
      progress: this.item.progress
    }
  },
  computed: {
    fontColor() {
      if (this.progress < 30) return "red";
      if (this.progress < 80) return "orange";
      return "green";
    }
  },
  filters: {
    toString(value) {
      return value.toLocaleString("ru", {month: "long", day: "numeric", year: "numeric", hour: "2-digit", minute: "2-digit", second:"2-digit"});
    }
  },
  methods: {
    updateTask() {
      const updated = {
        title: this.title,
        description: this.description,
        id: this.item.id,
        progress: this.progress
      }
      this.$emit('update-task', updated);
      this.isEdit = false;
    },
    cancel() {
      this.title = this.item.title;
      this.description = this.item.description;
      this.progress = this.item.progress;
      this.isEdit = false;
    }
  }
}
</script>

<style lang="less">
  .task {
    padding: 15px 0;
  }

  .task__wrapper, .task__buttons {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: flex-start;
  }

  .task__wrapper--top {
    width: 100%;
  }

  .task__wrapper--bottom {
    flex-wrap: wrap;
  }

  .task__progress {
    width: 120px;
    margin: 5px 0;
    padding: 0;

    & span {
      display: inline-block;
      width: 40px;
    }
  }

  .task__range {
    cursor: pointer;

    &:focus {
      outline: default;
    }
  }

  .task__edit-buttons {
    margin-top: 10px;
    display: flex;
    flex-direction: row;
    justify-content: flex-start;
  }

  .task__edit, .task__delete, .task__update, .task__cancel {
    font-size: 17px;

    cursor: pointer;
    background: none;
    border: none;
  }

  .task__update {
    padding: 5px 12px;
    margin-right: 20px;

    color: white;

    background-color: #052749;
    border-color: transparent;
    border-radius: 4px;
  }

  .task__cancel {
    color: inherit;

    text-decoration: underline;
  }

  .task__title {
    display: block;
    margin: 0;
    margin-bottom: 5px;
    padding: 0;
    text-align: start;
    font-size: 23px;
    font-weight: 700;
  }

  .task__category {
    margin: 0;
    padding: 0;
    text-align: start;
    font-size: 17px;
  }

  .task__description {
    margin: 15px 0;
    padding: 0;
    text-align: start;
  }

  .task__date {
    margin: 5px 0;
    padding: 0;
    text-align: start;
    font-size: 14px;
    font-weight: 700;
    color: #8F99A3;
  }

  .task__text {
    display: block;
    box-sizing: border-box;
    width: 100%;
    margin: 0;
    margin-bottom: 10px;
    padding: 8px 10px;

    font-size: 17px;

    cursor: text;

    box-shadow: inset 0 1px 2px rgba(10,10,10,.1);
    border: 1px solid transparent;
    border-color: #DAD8D9;
    border-radius: 4px;
  }

  .task__text--area {
    margin-top: 10px;
  }
</style>
