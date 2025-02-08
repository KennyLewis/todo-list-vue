<template>
  <div>
    <h1>Todo List</h1>
    <input type="text" v-model="todoInput" @keyup.enter="addTodoItem(todoInput)" />
    <button @click="addTodoItem(todoInput)">Add</button>
    <ul class="mt-2">
      <li v-for="todoItem in todoItems" :key="todoItem.id">
        <input type="checkbox" :id="todoItem.id" v-model="todoItem.completed" @click="toggleCompleted(todoItem.id)" />
        <label for="todo.id" :class="{ strikethrough: todoItem.completed }">{{ todoItem.title }}</label>
        <button @click="deleteTodoItem(todoItem.id)">Delete</button>
      </li>
    </ul>
    <button @click="deleteCompletedTodoItems()">Delete Completed Todo Items</button>
  </div>
</template>

<script lang="ts">

import { v4 as uuidv4 } from 'uuid';

interface TodoItem {
  id: string
  title: string
  completed: boolean
}

const initialTodoItems: TodoItem[] = [
  { id: uuidv4(), title: 'Learn Vue 3', completed: false },
  { id: uuidv4(), title: 'Learn React', completed: true },
  { id: uuidv4(), title: 'Build a project', completed: false },
]

const todoInput = '';

export default {
  name: 'TodoListView',
  data() {
    const todoItems = this.getLocalStorage() as TodoItem[];
    return {
      todoInput: todoInput,
      todoItems: todoItems
    }
  },
  methods: {
    addTodoItem(title: string) {
      if (!title) {
        return;
      }

      const id = uuidv4();
      this.todoItems.push({ id, title, completed: false });
      this.todoInput = '';
      this.saveLocalStorage();
    },
    deleteTodoItem(id: string) {
      this.todoItems = this.todoItems.filter((todoItem) => todoItem.id !== id);
      this.saveLocalStorage();
    },
    deleteCompletedTodoItems() {
      this.todoItems = this.todoItems.filter((todoInput) => !todoInput.completed);
      this.saveLocalStorage();
    },
    toggleCompleted(id: string) {
      const todo = this.todoItems.find((todoItem) => todoItem.id === id)
      if (todo) {
        todo.completed = !todo.completed
      }
      this.saveLocalStorage();
    },
    getLocalStorage(): TodoItem[] {
      const todoItems = localStorage.getItem('todoItems')
      if (todoItems) {
        return JSON.parse(todoItems);
      }

      return initialTodoItems;
    },
    saveLocalStorage() {
      localStorage.setItem('todoItems', JSON.stringify(this.todoItems))
    },
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
