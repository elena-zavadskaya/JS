<template>
  <li :class="['todo-item', { completed: todo.completed }]">
    <span>{{ todo.text }}</span>
    <div>
      <button class="complete-btn" @click="completeTodo">
        {{ todo.completed ? "Отменить" : "Выполнено" }}
      </button>
      <button class="delete-btn" @click="deleteTodo">Удалить</button>
    </div>
  </li>
</template>

<script>
export default {
  props: ["todo"],
  methods: {
    completeTodo() {
      this.$emit("complete", this.todo.id);
    },
    deleteTodo() {
      this.$emit("delete", this.todo.id);
    },
  },
};
</script>

<style scoped>
.todo-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  margin: 5px 0;
  border: 1px solid #ccc;
  border-radius: 4px;
  background-color: #f9f9f9;
}

.todo-item.completed {
  background-color: #d4edda;
  text-decoration: line-through;
}

button {
  padding: 5px 10px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.complete-btn {
  background-color: #78AF2C;
  color: white;
}

.complete-btn:hover {
  background-color: #589800;
}

.delete-btn {
  background-color: #BF4630;
  color: white;
}

.delete-btn:hover {
  background-color: #A61A00;
}
</style>


