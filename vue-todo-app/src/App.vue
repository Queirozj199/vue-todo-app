<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([
  { id: 1, text: 'Aprender Vue 3', done: false },
  { id: 2, text: 'Criar app de tarefas', done: false },
])

const addTodo = () => {
  const text = newTodo.value.trim()
  if (!text) return
  todos.value.push({
    id: Date.now(),
    text,
    done: false,
  })
  newTodo.value = ''
}

const removeTodo = (id) => {
  todos.value = todos.value.filter((todo) => todo.id !== id)
}

const remaining = computed(() => todos.value.filter((todo) => !todo.done).length)
</script>

<template>
  <main class="app">
    <h1>Todo App</h1>

    <form @submit.prevent="addTodo" class="todo-form">
      <input
        v-model="newTodo"
        type="text"
        placeholder="Nova tarefa"
      />
      <button type="submit">Adicionar</button>
    </form>

    <section class="todo-info">
      <p>Tarefas restantes: {{ remaining }}</p>
    </section>

    <ul class="todo-list">
      <li v-for="todo in todos" :key="todo.id" :class="{ done: todo.done }">
        <label>
          <input type="checkbox" v-model="todo.done" />
          <span>{{ todo.text }}</span>
        </label>
        <button @click="removeTodo(todo.id)">Excluir</button>
      </li>
    </ul>
  </main>
</template>

<style scoped>
.app {
  max-width: 520px;
  margin: 40px auto;
  padding: 0 16px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  margin-bottom: 24px;
}

.todo-form {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.todo-form input {
  flex: 1;
  padding: 10px;
  font-size: 1rem;
}

.todo-form button {
  padding: 10px 16px;
  cursor: pointer;
}

.todo-info {
  margin-bottom: 16px;
}

.todo-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.todo-list li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 6px;
  margin-bottom: 10px;
}

.todo-list li.done span {
  text-decoration: line-through;
  color: #666;
}

.todo-list button {
  background: #e74c3c;
  color: white;
  border: none;
  padding: 8px 12px;
  border-radius: 4px;
  cursor: pointer;
}
</style>
