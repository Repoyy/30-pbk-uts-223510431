<template>
    <div>
      <header>
        <div class="button-container">
          <button @click="toggleMenu('posts')">Post</button>
          <button @click="toggleMenu('todos')">Todos</button>
        </div>
      </header>
          
      <main>
        <slot name="posts" v-if="selectedMenu === 'posts'"></slot>
        <slot name="todos" v-else-if="selectedMenu === 'todos'"></slot>
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
        loading: true
      };
    },
    methods: {
      toggleMenu(menu) {
        this.selectedMenu = menu;
      }
    },
    mounted() {
      setTimeout(() => {
        this.loading = false;
      }, 1000);
    }
  };
  </script>
  
  <style scoped>
  header {
    background-color: #4CAF50;
    padding: 10px 0;
    text-align: center;
    border-bottom: 2px solid #2e7d32; 
  }
  
  .button-container {
    display: flex;
    justify-content: center;
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
  </style>
  