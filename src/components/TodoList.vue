<script setup>
import { ref, onMounted } from 'vue';

const todo = ref('')
const todos = ref([])
let id = 0;

function addTodo() {
  todos.value.unshift({ text: todo.value, id: id++ })
  localStorage.setItem('todos', JSON.stringify(todos.value))
  todo.value = ''
}

function removeTodo(i) {
  todos.value = todos.value.filter(t => t.id !== i)
  localStorage.setItem('todos', JSON.stringify(todos.value))
}

onMounted(() => {
  const data = JSON.parse(localStorage.getItem('todos') ?? '[]')
  if (data.length > 0) {
    todos.value = data;
    id = todos.value[0].id + 1;
  }
})
</script>

<template>
  <div class="max-w-xl mx-auto mt-14 py-6">
    <form class="flex gap-x-2 mb-5" @submit.prevent="addTodo">
      <input class="flex-1 border rounded h-12 px-3 outline-none" type="text" v-model="todo">
      <button class="flex items-center px-5 h-12 rounded bg-blue-600 text-white cursor-pointer disabled:bg-blue-200 disabled:cursor-default transition-colors" type="submit" :disabled="todo == ''">Ekle</button>
    </form>
    <ul class="flex flex-col gap-y-2" v-auto-animate>
      <li class="flex justify-between p-4 rounded bg-blue-100 text-blue-700" v-for="t in todos" :key="t.id">
        <p>{{ t.text }}</p>
        <span class="h-7 rounded px-3 bg-red-600 text-white cursor-pointer" @click="removeTodo(t.id)">x</span>
      </li>
    </ul>
  </div>
</template>
