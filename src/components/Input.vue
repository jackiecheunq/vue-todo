<script setup>
import { computed, inject, ref, watchEffect } from 'vue'
import classes from './Input.module.scss'
const tasks = inject('tasks')
const page = inject('page')
const checked = ref(false)


const filteredTasks = () =>
  tasks.value.filter((task) => task.status === page.value || page.value === 'all')

const computedFilteredTasks = computed(filteredTasks)

watchEffect(() => {
  if (computedFilteredTasks.value.every((task) => task.status === "completed")) {
    checked.value = true
  } else {
    checked.value = false
  }
})



const strategies = {
  completed: (task) => {
    task.status = 'active'
  },
  active: (task) => {
    task.status = 'completed'
  }
}

const taskStatusSwitcher = (task) => {
  strategies[task.status](task)
}
const switchAllTaskStatus = (event) => {
  if (event.target.checked) {
    filteredTasks().forEach(strategies.active)
  } else {
    filteredTasks().forEach(strategies.completed)
  }
}
</script>
<script>
export default {
  name: 'TodoInput'
}
</script>
<template>
  <section>
    <div v-if="computedFilteredTasks.length > 0">
      <input id="toggle-all" v-model="checked" class="toggle-all" type="checkbox" @change="switchAllTaskStatus" />
      <label for="toggle-all">Mark all as <span>complete</span></label>
    </div>

    <ul :class="classes.todoList">
      <li v-for="(task, index) in filteredTasks()" :key="index" :class="classes.listItem">
        <div v-if="page !== 'completed'">
          <button :class="classes.finishButton" @click="taskStatusSwitcher(task)">✓</button>
        </div>
        <p :class="[classes.taskContent, task.status === 'completed' && classes.crossedText]">
          {{ task.content }}
        </p>
        <div>
          <button :class="classes.deleteButton" @click="tasks.splice(
              tasks.findIndex((curTask) => curTask.id === task.id),
              1
            )
            ">
            X
          </button>
        </div>
      </li>
    </ul>
  </section>
</template>
