<script setup>
import { inject, ref } from 'vue'
const tasks = inject('tasks')
const input = ref('')
const inputHandler = () => {
  if (input.value.length > 0) {
    tasks.value.push({ content: input.value, status: 'active', id: Date.now() })
    input.value = ''
  }
}
</script>
<script>
export default {
  name: 'TodoHeader'
}
</script>

<template>
  <header class="header">
    <h1>todos</h1>
    <input
      class="new-todo"
      placeholder="What needs to be done?"
      autofocus
      v-model.trim="input"
      @keyup.enter="inputHandler"
    />
  </header>
</template>
