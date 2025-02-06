<template>
  <div>
    <h1>Todo List</h1>
    <input class="form-control" type="text" placeholder="Add Todo" />
    <ul class="mt-2">
      <li v-for="todoItem in todoItems" :key="todoItem.id">
        <input type="checkbox" :id="todoItem.id" v-model="todoItem.completed" @click="toggleCompleted(todoItem.id)" />
        <label for="todo.id" :class="{ strikethrough: todoItem.completed }">{{ todoItem.title }}</label>
        <button @click="deleteTodo(todoItem.id)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
const initialTodoItems = [
  { id: 1, title: 'Learn Vue 3', completed: false },
  { id: 2, title: 'Learn React', completed: true },
  { id: 3, title: 'Build a project', completed: false },
]

export default {
  name: 'TodoListView',
  data() {
    return {
      todoItems: initialTodoItems,
    }
  },
  methods: {
    toggleCompleted(id: number) {
      const todo = this.todoItems.find((todo) => todo.id === id)
      if (todo) {
        todo.completed = !todo.completed
      }
    },
    deleteTodo(id: number) {
      this.todoItems = this.todoItems.filter((todo) => todo.id !== id)
    }
  },
}
</script>

<style scoped>
ul {
  list-style-type: none;
  padding-left: 0;
  margin-top: .5rem;
}

li {
  display: flex;
  align-items: center;
  margin-bottom: .5rem;
}

input[type="checkbox"] {
  margin-right: .5rem;
}

button {
  margin-left: .5rem;
}

.strikethrough {
  text-decoration: line-through;
}
</style>
