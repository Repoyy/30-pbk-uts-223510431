<template>
  <div id="app">
    <Main>
      <template v-slot:posts>
        <Post />
      </template>
      <template v-slot:todos>
        <Todos 
          :tasks="tasks" 
          @add-task="addTask" 
          @update-task="updateTask" 
          @delete-task="deleteTask" 
          @complete-task="completeTask"
        />
      </template>
    </Main>
  </div>
</template>

<script>
import Main from './components/Main.vue';
import Post from './components/Post.vue';
import Todos from './components/Todos.vue';

export default {
  name: 'App',
  components: {
    Main,
    Post,
    Todos
  },
  data() {
    return {
      tasks: []
    };
  },
  methods: {
    addTask(task) {
      this.tasks.push(task);
    },
    updateTask(index) {
      this.tasks[index].editing = false;
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    completeTask(index) {
      this.$set(this.tasks[index], 'completed', true);
    }
  }
};
</script>

<style>
.container {
  max-width: 600px;
  margin: 0 auto;
  border: 1px solid #ccc;
  padding: 20px;
}

.container h1 {
  color: #4CAF50; /* Set the text color to green */
}

.button-container {
  display: flex;
  justify-content:center;
}

button {
  background-color: #ffffff;
  border: none;
  padding: 10px 20px;
  margin: 0 10px;
  border-radius: 5px;
  cursor: pointer;
  font-size: 16px;
  color: #000; /* Set the text color to black */
}

button:hover {
  background-color: #f0f0f0;
}
</style>
