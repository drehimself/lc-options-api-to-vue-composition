<template>
  <div>
    <h2 class="text-center">Main App</h2>
    <TodoList title="Todo List" />
  </div>
</template>

<script>
import TodoList from './components/TodoList.vue'

export default {
  components: { TodoList },
}
</script>

<style>
.text-center {
  text-align: center;
}
</style>
