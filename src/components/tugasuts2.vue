<script setup>
import { ref, computed } from 'vue'

let id = 0

const newTodo = ref('')
const hideCompleted = ref(false)
const todos = ref([
  { id: id++, text: 'Treadmill', done: false },
  { id: id++, text: 'Dumbbell', done: false },
  { id: id++, text: 'Bench Press', done: true },
  { id: id++, text: 'Pull-up Bar', done: false }
])
//membuat delate and update
//menambahkan 4 data list
//menambahkan filter untuk menyembunyikan alat gym yang sudah selesai 
//membuat antarmuka modern dengan latar buram dan tema hijau
const filteredTodos = computed(() => {
  return hideCompleted.value
    ? todos.value.filter((t) => !t.done)
    : todos.value
})

function addTodo() {
  todos.value.push({ id: id++, text: newTodo.value, done: false })
  newTodo.value = ''
}

function removeTodo(todo) {
  todos.value = todos.value.filter((t) => t !== todo)
}
</script>

<template>
  <form @submit.prevent="addTodo">
    <input v-model="newTodo" required placeholder="Nama alat gym">
    <button>Add Alat</button>
  </form>

  <ul>
    <li v-for="todo in filteredTodos" :key="todo.id" class="todo-item">
      <div class="todo-content">
        <input type="checkbox" v-model="todo.done">
        <span :class="{ done: todo.done }">{{ todo.text }}</span>
      </div>
      <button @click="removeTodo(todo)">X</button>
    </li>
  </ul>

  <button @click="hideCompleted = !hideCompleted">
    {{ hideCompleted ? 'Tampilkan Semua' : 'Sembunyikan yang Selesai' }}
  </button>
</template>

<style>
body {
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background: url('https://images.unsplash.com/photo-1571019613454-1cb2f99b2d8b') no-repeat center center fixed;
  background-size: cover;
  margin: 0;
  padding: 2rem;
  min-height: 100vh;
}

form, ul, button {
  backdrop-filter: blur(6px);
  background-color: rgba(255, 255, 255, 0.75);
  border-radius: 12px;
  padding: 1em;
  box-shadow: 0 4px 10px rgba(0,0,0,0.2);
  margin-bottom: 1rem;
}

form {
  display: flex;
  gap: 0.5em;
}

input[type="text"] {
  flex: 1;
  padding: 0.6em;
  border: 2px solid #4CAF50;
  border-radius: 8px;
  font-size: 1em;
}

button {
  padding: 0.6em 1.2em;
  background: linear-gradient(135deg, #4CAF50, #388E3C);
  color: white;
  border: none;
  border-radius: 8px;
  font-weight: bold;
  cursor: pointer;
  transition: 0.3s ease;
}

button:hover {
  background: linear-gradient(135deg, #45a049, #2e7d32);
  transform: scale(1.05);
}

ul {
  list-style: none;
  padding: 0;
}

.todo-item {
  background-color: rgba(255, 255, 255, 0.85);
  padding: 0.75em 1em;
  margin-bottom: 0.5em;
  border-radius: 12px;
  box-shadow: 0 4px 6px rgba(0,0,0,0.1);
  display: flex;
  align-items: center;
  justify-content: space-between;
  transition: 0.2s ease-in-out;
}

.todo-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 10px rgba(0,0,0,0.15);
}

.todo-content {
  display: flex;
  align-items: center;
  flex: 1;
}

.done {
  text-decoration: line-through;
  color: #888;
}

input[type="checkbox"] {
  margin-right: 0.8em;
  transform: scale(1.2);
}
</style>
