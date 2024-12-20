<template>
  <div class="container">
    <h1>Список дел</h1>
    <add-todo @add="addTodo" />
    <ul>
      <todo-item
        v-for="todo in todos"
        :key="todo.id"
        :todo="todo"
        @delete="deleteTodo"
        @complete="markComplete"
      />
    </ul>
  </div>
</template>

<script>
import AddTodo from "./AddTodo.vue";
import TodoItem from "./TodoItem.vue";

export default {
  components: { AddTodo, TodoItem },
  data() {
    return {
      todos: [
        { id: 1, text: "Купить продукты", completed: false },
        { id: 2, text: "Сходить в спортзал", completed: false },
      ],
    };
  },
  methods: {
    addTodo(text) {
      const newId = this.todos.length ? this.todos[this.todos.length - 1].id + 1 : 1;
      this.todos.push({ id: newId, text, completed: false });
    },
    deleteTodo(id) {
      this.todos = this.todos.filter((todo) => todo.id !== id);
    },
    markComplete(id) {
      const todo = this.todos.find((todo) => todo.id === id);
      if (todo) todo.completed = !todo.completed;
    },
  },
};
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@300;600&display=swap');

* {
  margin: 0;
  padding: 0;
}

body {
  font-family: 'Montserrat', sans-serif;
  height: 100vh;
  background: linear-gradient(339deg, rgba(51, 21, 176, 1) 0%, rgba(253, 187, 45, 1) 100%);
  justify-content: center;
  align-items: center;
  text-align: center;
  margin: 0;
  padding: 20px;
}

.container {
  width: 600px;
  margin: 0 auto;
  background: #fff;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

ul {
  list-style: none;
  padding: 0;
}
</style>