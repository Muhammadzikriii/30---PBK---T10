<template>
  <div class="todo-app">
    <h1>My Todo List</h1>
    <div class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" type="text" placeholder="Add a new todo">
      <button @click="addTodo" class="add-btn">+</button>
    </div>
    <transition-group name="fade">
      <div v-for="(todo, index) in todos" :key="index" class="todo-item">
        <span>{{ todo }}</span>
        <button @click="removeTodo(index)" class="delete-btn">x</button>
      </div>
    </transition-group>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: []
    };
  },
  methods: {
    addTodo() {
      if (this.newTodo.trim() !== '') {
        this.todos.push(this.newTodo);
        this.newTodo = '';
      }
    },
    removeTodo(index) {
      this.todos.splice(index, 1);
    }
  }
};
</script>

<style scoped>
.todo-app {
  font-family: Arial, sans-serif;
  max-width: 400px;
  margin: 0 auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #333;
}

.input-container {
  margin-bottom: 20px;
}

input[type="text"] {
  width: 70%;
  padding: 10px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.add-btn {
  width: 30%;
  padding: 10px;
  font-size: 16px;
  border: none;
  background-color: #4caf50;
  color: #fff;
  border-radius: 4px;
  cursor: pointer;
}

.add-btn:hover {
  background-color: #45a049;
}

.todo-item {
  display: flex;
  align-items: center;
  background-color: #f9f9f9;
  border-radius: 4px;
  margin-bottom: 10px;
  padding: 10px;
}

.todo-item span {
  flex-grow: 1;
}

.delete-btn {
  background-color: #f44336;
  color: #fff;
  border: none;
  padding: 5px 10px;
  border-radius: 50%;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.delete-btn:hover {
  background-color: #d32f2f;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity 0.5s;
}

.fade-enter, .fade-leave-to {
  opacity: 0;
}
</style>
