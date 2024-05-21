<template>
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
  </template>
  
  <script>
  export default {
    props: {
      tasks: Array
    },
    data() {
      return {
        newTask: '',
        showIncomplete: true
      };
    },
    methods: {
      addTask() {
        if (this.newTask.trim() !== '') {
          this.$emit('add-task', { title: this.newTask, completed: false, editing: false });
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
        this.$emit('update-task', index);
      },
      deleteTask(index) {
        this.$emit('delete-task', index);
      },
      toggleShowIncomplete() {
        this.showIncomplete = !this.showIncomplete;
      },
      completeTask(index) {
        this.$emit('complete-task', index);
      }
    },
    computed: {
      incompleteTasks() {
        if (this.showIncomplete) {
          return this.tasks.filter(task => !task.completed);
        } else {
          return this.tasks;
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .container {
    max-width: 600px;
    margin: 0 auto;
    border: 1px solid #ccc;
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
  
  .task-text {
    flex: 1;
    color: #000200;
  }
  
  .task input[type="checkbox"] {
    margin-right: 10px;
  }
  
  .edit-mode {
    display: flex;
    justify-content: space-between;
  }
  
  .edit-input {
    flex: 1;
    margin-right: 10px;
  }
  
  .update-btn {
    background-color: #4CAF50;
  }
  
  .cancel-btn {
    background-color: #f44336;
  }
  
  .edit-btn {
    background-color: #ff9800;
  }
  
  .delete-btn {
    background-color: #f44336;
  }
  </style>
  