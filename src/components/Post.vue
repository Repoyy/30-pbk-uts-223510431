<template>
    <div class="post-container">
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
  </template>
  
  <script>
  export default {
    data() {
      return {
        users: [],
        selectedUser: null,
        userPosts: []
      };
    },
    methods: {
      async fetchUsers() {
        const response = await fetch('https://jsonplaceholder.typicode.com/users');
        this.users = await response.json();
      },
      async fetchPosts() {
        const response = await fetch(`https://jsonplaceholder.typicode.com/posts?userId=${this.selectedUser}`);
        this.userPosts = await response.json();
      }
    },
    mounted() {
      this.fetchUsers();
    },
    watch: {
      selectedUser() {
        this.fetchPosts();
      }
    }
  };
  </script>
  
  <style scoped>
  .post-container {
    padding: 20px;
  }
  
  .table-container {
    margin-top: 20px;
    border: 1px solid #ffffff;
    border-radius: 5px;
    overflow: hidden;
  }
  
  .post-table {
    width: 100%;
    border-collapse: collapse;
  }
  
  .post-table th, .post-table td {
    padding: 10px;
    border: 1px solid #ffffff;
    text-align: left;
    color: #4CAF50;
  }

  
  
  .post-table th {
    background-color: #ffffff;
  }
  
  .post-table tr:nth-child(even) {
    background-color: #070505;
  }
  
  strong {
    color: #ffffff;
  }
  </style>
  