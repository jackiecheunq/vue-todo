<script setup>
import TodoHeader from './components/Header.vue'
import TodoInput from './components/Input.vue'
import TodoFooter from './components/Footer.vue'
import { ref, provide, watch, onMounted } from 'vue'

const tasks = ref([])
const page = ref('all')
provide('tasks', tasks)
provide('page', page)

onMounted(() => {
  tasks.value = JSON.parse(localStorage.todo || '[]')
})

watch(
  tasks,
  () => {
    const data = JSON.stringify(tasks.value)
    localStorage.todo = data
  },
  { deep: true }
)
</script>

<template>
  <TodoHeader />
  <TodoInput />
  <TodoFooter />
</template>
