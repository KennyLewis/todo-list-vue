<template>
  <div>
    <h1>Todo List</h1>
    <input class="form-control" type="text" placeholder="Add Todo" />
    <ul class="mt-2">
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" :id="todo.id" v-model="todo.completed" @click="toggleCompleted(todo.id)" />
        <label class="pl-2" :for="todo.id" :class="{ strikethrough: todo.completed }">{{ todo.title }}</label>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
const initialTodos = [
  { id: 1, title: 'Learn Vue 3', completed: false },
  { id: 2, title: 'Learn React', completed: true },
  { id: 3, title: 'Build a project', completed: false },
]

export default {
  name: 'TodoListView',
  data() {
    return {
      todos: initialTodos,
    }
  },
  methods: {
    toggleCompleted(id: number) {
      const todo = this.todos.find((todo) => todo.id === id)
      if (todo) {
        todo.completed = !todo.completed
      }
    },
  },
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding: 0;
}

.mt-2 {
  margin-top: 0.5rem;
}

.pl-2 {
  padding-left: 0.5rem;
}

.strikethrough {
  text-decoration: line-through;
}
</style>
