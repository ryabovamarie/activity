<template>
  <div id="app">
    <h1 class="page-title">Activity planner</h1>
    <Filters @update-filter="updateFilter"/>
    <main class="page-main">
      <AddTask class="page-main__new"
        v-bind:categories=categories
        @add-task="addTask"
      />
      <TaskList class="page-main__list"
        v-bind:tasks=filteredTasks
        @remove-task="removeTask"
        @update-task="updateTask"
      />
    </main>
  </div>
</template>

<script>
import Filters from '@/components/Filters';
import AddTask from '@/components/AddTask';
import TaskList from '@/components/TaskList';

export default {
  name: 'app',
  data() {
    return {
      tasks: [],
      categories: [
        "Книги",
        "Фильмы",
        "Учеба",
        "Работа"
      ],
      filter: 'all'
    }
  },
  components: {
    Filters, AddTask, TaskList
  },
  methods: {
    removeTask(id) {
      this.tasks = this.tasks.filter(t => t.id !== id);
      localStorage.removeItem(id);
    },
    addTask(item) {
      this.tasks.push(item);
      localStorage.setItem(item.id, JSON.stringify(item));
    },
    updateTask(updated) {
      this.tasks = this.tasks.map(t => {
        if (t.id === updated.id) {
          t.title = updated.title;
          t.description = updated.description;
          t.progress = updated.progress;
          t.date = new Date();
          localStorage.setItem(t.id, JSON.stringify(t));
        }
        return t;
      });
    },
    updateFilter(filter) {
      this.filter = filter;
    }
  },
  computed: {
    filteredTasks() {
      if (this.filter === 'all') {
        return this.tasks;
      }

      if (this.filter === 'finished') {
        return this.tasks.filter(t => t.progress === 100);
      }

      if (this.filter === 'in-progress') {
        return this.tasks.filter(t => t.progress < 100 && t.progress > 0);
      }

      if (this.filter === 'not-started') {
        return this.tasks.filter(t => t.progress === 0);
      }
    }
  },
  mounted() {
    for (let i = 0; i < localStorage.length; i++) {
      let key = localStorage.key(i);
      if (!isNaN(parseInt(key)) && localStorage[key] !== null) {
        this.tasks.push(JSON.parse(localStorage[key], (key, value) => {
          if (key == "date") return new Date(value);
          return value;
        }));
      }
    }
  }
}
</script>

<style>
#app {
  margin: 0;
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

.page-title {
  padding: 20px 0;
  margin: 0;
  background-color: white;
}

.page-main {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;

  margin: 0 auto;
  max-width: 1000px;
  margin-top: 30px;
  padding: 0 15px;
}

.page-main__new {
  flex-grow: 1;
  margin-right: 15px;
  min-width: 200px;
  width: 200px;
  box-sizing: border-box;
}

.page-main__list {
  flex-grow: 3;

  min-width: 250px;
  width: 400px;
  box-sizing: border-box;
}

@media (max-width: 500px) {
  .page-main {
    flex-direction: column;
    padding: 0 20px;
  }

  .page-main__new {
    width: 100%;
    margin-right: 0;
    margin-bottom: 15px;
  }

  .page-main__list {
    width: 100%;
    margin: 0;
  }
}
</style>
