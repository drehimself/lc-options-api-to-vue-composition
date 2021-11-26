<template>
  <div class="todo-container">
    <h3>{{ capitalize(props.title) }}</h3>
    <TodoDescription />
    <div class="todo-input-container">
      <form action="#" @submit.prevent="addTodo">
        <input type="text" v-model="todoInput" class="todo-input" />
      </form>
    </div>

    <div v-if="todos.length > 0">
      <ul class="todo-list-container">
        <li v-for="todo in todos" :key="todo.id">
          <div>
            <input type="checkbox" v-model="todo.complete" />
            <span :class="{ complete: todo.complete }">{{ todo.title }}</span>
          </div>
          <button @click="deleteTodo(todo.id)">x</button>
        </li>
      </ul>
      <div class="todos-remaining">{{ remaining }} item(s) remaining</div>
    </div>
    <div class="no-todos" v-else>Add a todo...</div>
  </div>
</template>

<script setup>
import TodoDescription from './TodoDescription.vue'
import { capitalize } from '../helpers/string-helpers'
import { ref, computed, onMounted, watch } from 'vue'

const props = defineProps({
  title: String,
})
onMounted(() => {
  console.log('component mounted')
  console.log('The prop title is: ' + props.title)
})

const newId = ref(3)

function addTodo() {
  todos.value.push({
    id: newId.value,
    title: todoInput.value,
    complete: false,
  })

  newId.value++
  todoInput.value = ''
}

const todoInput = ref('')
const todos = ref([
  {
    id: 1,
    title: 'Finish Vue Screencast',
    complete: false,
  },
  {
    id: 2,
    title: 'Take over world',
    complete: false,
  },
])

watch(newId, (newId, oldId) => {
  console.log('Id is now: ' + newId)
})

function deleteTodo(id) {
  todos.value = todos.value.filter(todo => id !== todo.id)
}

const remaining = computed(() => {
  return todos.value.filter(todo => !todo.complete).length
})
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  font-size: 20px;
  background: lightgray;
  display: flex;
  flex-direction: column;
  flex: 1;
  min-height: 100vh;
}

.todo-container {
  width: 400px;
  margin: 40px auto;
  background: white;
  border-radius: 10px;
  padding: 20px;
}

h3 {
  margin-top: 0 0 20px 0;
}

.todo-input-container {
  width: 500px;
  max-width: 100%;
}

.todo-input {
  border: none;
  border: 1px solid lightgray;
  border-radius: 5px;
  padding: 10px 0px;
  padding-left: 10px;
  width: 100%;
}

.todo-list-container {
  padding-left: 4px;
  margin-top: 30px;
}

.todo-list-container li {
  list-style-type: none;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.todo-list-container span {
  margin-left: 10px;
}

.no-todos {
  margin-top: 20px;
}

.complete {
  text-decoration: line-through;
}

.todos-remaining {
  font-size: 14px;
}
</style>
