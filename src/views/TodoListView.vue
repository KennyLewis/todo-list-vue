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
interface TodoItem {
  id: number
  title: string
  completed: boolean
}

const initialTodoItems: TodoItem[] = [
  { id: 1, title: 'Learn Vue 3', completed: false },
  { id: 2, title: 'Learn React', completed: true },
  { id: 3, title: 'Build a project', completed: false },
]
export default {
  name: 'TodoListView',
  data() {
    const todoItems = this.getLocalStorage() as TodoItem[];
    return {
      todoItems: todoItems
    }
  },
  methods: {
    toggleCompleted(id: number) {
      const todo = this.todoItems.find((todoItem) => todoItem.id === id)
      if (todo) {
        todo.completed = !todo.completed
      }
      this.saveLocalStorage();
    },
    deleteTodo(id: number) {
      this.todoItems = this.todoItems.filter((todoItem) => todoItem.id !== id);
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
