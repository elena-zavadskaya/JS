<template>
  <div>
    <input v-model="newTodo" type="text" placeholder="Введите новое дело" />
    <button @click="addTodo">Добавить</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: ""
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim()) {
        this.$emit("add", this.newTodo.trim());
        this.newTodo = "";
      }
    },
  },
};
</script>

<style scoped>
input {
  padding: 10px;
  width: calc(100% - 100px);
  margin-right: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 10px;
  background-color: #FFCA00;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #BFA130;
}
</style>


