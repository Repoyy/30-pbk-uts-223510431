<template>
  <div>
    <header>
      <div class="button-container">
        <button @click="toggleMenu('posts')">Post</button>
        <button @click="toggleMenu('todos')">Todos</button>
      </div>
    </header>
        
    <main>
      <div v-if="selectedMenu === 'posts'" class="post-container">
        <select v-model="selectedUser" @change="fetchPosts">
          <option v-for="user in users" :value="user.id">{{ user.name }}</option>
        </select>
        <div class="table-container">
          <table class="post-table">
            <thead>
              <tr>
                <th>Judul</th>
                <th>Isi</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="post in userPosts" :key="post.id">
                <td><strong>{{ post.title }}</strong></td>
                <td>{{ post.body }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
  
      <div v-else-if="selectedMenu === 'todos'">
        <div class="container">
          <h1 style="text-align: center;">Daftar Tugas</h1>
          <div style="display: flex;">
            <input type="text" v-model="newTask" placeholder="Tambahkan tugas baru...">
            <button @click="addTask">Tambah</button>
          </div>
  
          <div v-for="(task, index) in incompleteTasks" :key="index" class="task" :class="{ 'completed': task.completed }">
            <input type="checkbox" v-model="task.completed" class="checkbox" @change="completeTask(index)">
            <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
            <div v-else class="edit-mode">
              <input v-model="task.title" class="edit-input">
              <div class="button-group">
                <button class="update-btn" @click="updateTask(index)">Perbarui</button>
                <button class="cancel-btn" @click="cancelTask(index)">Batal</button>
              </div>
            </div>
            <div v-if="!task.editing" class="button-group">
              <button class="edit-btn" @click="editTask(index)">Edit</button>
              <button class="delete-btn" @click="deleteTask(index)">Hapus</button>
            </div>
          </div>
  
          <button class="filter-btn" @click="toggleShowIncomplete">
            {{ showIncomplete ? 'Tampilkan semua tugas' : 'Tampilkan hanya tugas yang belum selesai' }}
          </button>
        </div>
      </div>
    </main>

    <div v-if="loading" class="loading">
      <div class="icons">
        <i class="ri-arrow-left-s-line"></i>
        <i class="ri-arrow-right-s-line"></i>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      selectedMenu: 'posts',
      todos: [],
      users: [],
      selectedUser: null,
      userPosts: [],
      tasks: [],
      newTask: '',
      showIncomplete: true,
      loading: true
    };
  },
  methods: {
    async fetchUsers() {
      this.loading = true;
      const response = await fetch('https://jsonplaceholder.typicode.com/users');
      this.users = await response.json();
      this.loading = false;
    },
    async fetchPosts() {
      this.loading = true;
      const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
      this.userPosts = await response.json();
      this.loading = false;
    },
    toggleMenu(menu) {
      this.selectedMenu = menu;
    },
    addTask() {
      if (this.newTask.trim() !== '') {
        this.tasks.push({ title: this.newTask, completed: false, editing: false });
        this.newTask = '';
      }
    },
    cancelTask(index) {
      this.tasks[index].editing = false;
    },
    editTask(index) {
      this.tasks[index].editing = true;
    },
    updateTask(index) {
      this.tasks[index].editing = false;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    toggleShowIncomplete() {
      this.showIncomplete = !this.showIncomplete;
    },
    completeTask(index) {
      this.$set(this.tasks[index], 'completed', true);
    }
  },
  mounted() {
    this.fetchUsers();
    setTimeout(() => {
      this.loading = false;
    }, 1000);
  },
  computed: {
    incompleteTasks() {
      if (this.showIncomplete) {
        return this.tasks.filter(task => !task.completed);
      } else {
        return this.tasks;
      }
    }
  },
  watch: {
    selectedUser() {
      this.fetchPosts();
    },
  }
};
</script>

<style>
header {
  background-color: #4CAF50;
  padding: 10px 0;
  text-align: center;
  border-bottom: 2px solid #2e7d32; 
}

.button-container {
  display: flex;
  justify-content:center;
}

button {
  background-color: #fff;
  border: none;
  padding: 10px 20px;
  margin: 0 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background-color: #f0f0f0;
}

main {
  padding: 20px;
}

.task {
  border: 1px solid #0678fa;
  border-radius: 5px;
  padding: 10px;
  margin: 10px 0;
  background-color: #f9f9f9;
}

.completed {
  text-decoration: line-through;
}

.button-group {
  display: flex;
  justify-content: flex-end;
  margin-top: 10px;
}

button {
  background-color: #4CAF50;
  border: none;
  color: rgb(241, 241, 241);
  padding: 10px 20px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 5px;
}

button:hover {
  background-color: #45a049;
}


.filter-btn {
  background-color: #008CBA;
  color: rgb(0, 0, 0);
  border: none;
  border-radius: 5px;
  padding: 10px 20px;
  cursor: pointer;
  font-size: 16px;
  margin-top: 20px;
}

.filter-btn:hover {
  background-color: #005f6b;
}

.loading {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(255, 255, 255, 0.8);
  display: flex;
  justify-content: center;
  align-items: center;
}

.icons {
  font-size: 48px;
  color: #4CAF50;
  animation: spin 2s linear infinite;
}

@keyframes spin {
  0% { transform: rotate(0deg); }
  100% { transform: rotate(360deg); }
}

.container {
  max-width: 600px;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 20px;
}

.task-text {
  flex: 1;
  color: #000200;
}

.task input[type="checkbox"] {
  margin-right: 10px;
}
</style>
