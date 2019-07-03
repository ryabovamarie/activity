<template>
  <div class="tasks">
    <ul class="tasks__list">
      <Task class="tasks__item"
        v-for="item of tasks"
        v-bind:item=item
        @remove-task="removeTask"
        @update-task="updateTask"
      />
    </ul>
    <p class="tasks__total">Всего задач: {{tasks.length}}</p>
  </div>
</template>

<script>
import Task from '@/components/Task';

export default {
  props: ["tasks"],
  components: {
    Task
  },
  methods: {
    removeTask(id) {
      this.$emit('remove-task', id);
    },
    updateTask(updated) {
      this.$emit('update-task', updated);
    }
  }
}
</script>

<style>
  .tasks {
    padding: 0 25px;
    background-color: white;
    border: 1px solid #DAD8D9;
    border-radius: 4px;
  }

  .tasks__list {
    padding: 0;
    list-style: none;
  }

  .tasks__item {
    border-bottom: 1px solid #DAD8D9;
  }
</style>
