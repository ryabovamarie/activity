<template>
  <header class="page-header">
    <div class="page-header__wrapper">
      <button class="page-header__toggle" type="button" v-if="!opened" @click="opened = !opened">Открыть меню</button>
      <button class="page-header__toggle" type="button" v-if="opened" @click="opened = !opened">Закрыть меню</button>
      <form class="filters" v-if="show">
        <div class="filters__wrapper">
          <input class="filters__input" type="radio" name="filters" value="all" id="all" checked v-model="filter">
          <label class="filters__text" for="all">Все</label>
        </div>

        <div class="filters__wrapper">
          <input class="filters__input" type="radio" name="filters" value="in-progress" id="in-progress" v-model="filter">
          <label class="filters__text" for="in-progress">В процессе</label>
        </div>

        <div class="filters__wrapper">
          <input class="filters__input" type="radio" name="filters" value="finished" id="finished" v-model="filter">
          <label class="filters__text" for="finished">Завершенные</label>
        </div>

        <div class="filters__wrapper">
          <input class="filters__input" type="radio" name="filters" value="not-started" id="not-started" v-model="filter">
          <label class="filters__text" for="not-started">Не начатые</label>
        </div>
      </form>
    </div>
  </header>
</template>

<script>
export default {
  data() {
    return {
      opened: false,
      windowWidth: window.innerWidth,
      filter: 'all'
    }
  },
  mounted () {
    window.onresize = (event) => {
      this.windowWidth = window.innerWidth;
    };
  },
  computed: {
    show() {
      return this.opened || this.windowWidth > 500;
    }
  },
  watch: {
    filter(value) {
      this.$emit("update-filter", value);
    }
  }
}
</script>

<style lang="less">
  .page-header {
    background-color: white;
  }

  .page-header__wrapper {
    margin: 0 auto;
    max-width: 1000px;
    padding: 0 15px;
  }

  .page-header__toggle {
    display: none;
  }

  .filters {
    display: flex;
    box-sizing: border-box;
    border-top: 1px solid #cecece;
  }

  .filters__text {
    display: block;
    padding: 10px 15px;
    box-sizing: border-box;

    font-size: 20px;
    font-weight: 500;
    &:hover {
      cursor: pointer;
    }
  }

  .filters__input {
    display: none;

    &:checked + .filters__text {
      background-color: #cecece;

      &:hover {
        cursor: default;
      }
    }
  }

  @media (max-width: 500px) {
    .page-header__toggle {
      display: block;
      padding: 10px 0;
      width: 100%;
      box-sizing: border-box;

      color: #2c3e50;
      font-size: 20px;
      font-weight: 500;

      background:none;
      border: none;
      border-top: 1px solid #cecece;

      &:hover {
        cursor: pointer;
        background-color: #cecece;
      }
    }

    .page-header__wrapper {
      padding: 0;
      border-bottom: 1px solid #cecece;
    }

    .filters {
      flex-direction: column;
    }
  }
</style>
