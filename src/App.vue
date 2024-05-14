<template>
  <div class="todo-app" style="background-color: black; color: white;">
    <h1>My Todo List</h1>
    <div class="header">
      <button @click="showTodos" :class="{ 'active': activeTab === 'todos' }">Todos</button>
      <button @click="showPosts" :class="{ 'active': activeTab === 'posts' }">Posts</button>
    </div>
    <div v-show="activeTab === 'todos'" class="input-container">
      <input v-model="newTodo" @keyup.enter="addTodo" type="text" placeholder="Add a new todo" style="background-color: #333; color: white; border: 1px solid white;">
      <button @click="addTodo" class="add-btn">+</button>
    </div>
    <transition-group name="fade">
      <div v-show="activeTab === 'todos'" v-for="(todo, index) in todos" :key="index" class="todo-item">
        <span>{{ todo }}</span>
        <button @click="removeTodo(index)" class="delete-btn">x</button>
      </div>
    </transition-group>
    <div v-show="activeTab === 'posts'" class="post-container">
      <h2>Posts</h2>
      <div>
        <label for="userSelect">Select User:</label>
        <select v-model="selectedUserId" @change="fetchUserPosts" id="userSelect">
          <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
        </select>
      </div>
      <div v-if="loadingPosts" class="text-muted">Loading posts...</div>
      <div v-else>
        <div v-for="post in userPosts" :key="post.id" class="post-item">
          <h3>{{ post.title }}</h3>
          <p>{{ post.body }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      newTodo: '',
      todos: [],
      activeTab: 'todos',
      users: [],
      selectedUserId: null,
      userPosts: [],
      loadingPosts: false
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
    },
    showTodos() {
      this.activeTab = 'todos';
    },
    showPosts() {
      this.activeTab = 'posts';
      this.fetchUsers();
    },
    fetchUsers() {
      fetch('https://jsonplaceholder.typicode.com/users')
        .then(response => response.json())
        .then(data => {
          this.users = data;
          this.selectedUserId = this.users[0].id; // Select the first user by default
          this.fetchUserPosts(); // Fetch posts for the selected user
        })
        .catch(error => {
          console.error('Error fetching users:', error);
        });
    },
    fetchUserPosts() {
      this.loadingPosts = true;
      fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUserId}`)
        .then(response => response.json())
        .then(data => {
          this.userPosts = data;
          this.loadingPosts = false;
        })
        .catch(error => {
          console.error('Error fetching posts:', error);
          this.loadingPosts = false;
        });
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
}

.header {
  text-align: center;
  margin-bottom: 20px;
}

.header button {
  background-color: transparent;
  color: white;
  border: none;
  font-size: 16px;
  cursor: pointer;
}

.header button.active {
  font-weight: bold;
}

.input-container {
  margin-bottom: 20px;
}

.add-btn {
  width: 30%;
  padding: 10px;
  font-size: 16px;
  border: none;
  background-color: #fafafa;
  color: #080808;
  border-radius: 4px;
  cursor: pointer;
}

.add-btn:hover {
  background-color: #4caf50;
}

.todo-item {
  display: flex;
  align-items: center;
  background-color: #040404;
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

.post-container {
  color: white;
}

.post-item {
  background-color: #333;
  padding: 10px;
  margin-bottom: 10px;
  border-radius: 4px;
}

.post-item h3 {
  margin-top: 0;
}
</style>
