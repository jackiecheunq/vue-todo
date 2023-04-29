<script setup>
import classes from './Footer.module.scss'
import { inject } from 'vue'
const page = inject('page')
const tasks = inject('tasks')

const switchHandler = (pageId) => {
  page.value = pageId
}

const clearCompletedHandler = () => {
  tasks.value = tasks.value.filter((task) => task.status !== 'completed')
}
</script>

<script>
export default {
  name: 'TodoFooter'
}
</script>

<template>
  <footer class="footer">
    <span class="todo-count"></span>
    <span>Current Page: {{ page }}</span>
    <ul :class="classes.filters">
      <li>
        <button @click="switchHandler('all')">All</button>
      </li>
      <li>
        <button @click="switchHandler('active')">Active</button>
      </li>
      <li>
        <button @click="switchHandler('completed')">Completed</button>
      </li>
    </ul>
    <button class="clear-completed" @click="clearCompletedHandler">Clear completed</button>
  </footer>
</template>
