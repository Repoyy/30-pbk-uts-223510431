<script setup>
import HelloWorld from './components/HelloWorld.vue'
import { ref, computed } from 'vue';

const todos = ref([
  { id: 1, text: 'Belajar Pemrograman Berbasis Komponen', done: false },
  { id: 2, text: 'Mengerjakan Tugas', done: true },
  { id: 3, text: 'Bermain Game RPG', done: false }
]);

const newTodo = ref('');
const hideCompleted = ref(false);

const addTodo = () => {
  if (newTodo.value.trim() !== '') {
    todos.value.push({ id: Date.now(), text: newTodo.value, done: false });
    newTodo.value = '';
  }
};

const filteredTodos = computed(() => {
  return hideCompleted.value ? todos.value.filter(todo => !todo.done) : todos.value;
});

const removeTodo = todo => {
  const index = todos.value.indexOf(todo);
  if (index !== -1) {
    todos.value.splice(index, 1);
  }
};
</script>



<template>
  <div class="container mx-auto bg-slate-500 rounded-xl shadow border p-20 m-10">
      <div class="text-center">
          <h1 class="text-3xl font-bold pb-4">List Kegiatan</h1>
      </div>
      <div class="font-bold pb-4">
          <form @submit.prevent="addTodo">
          <input class="shadow-lg border-spacing-2 p-2 rounded-lg " placeholder="tambahkan list" v-model="newTodo">
          <button class="px-6 border rounded-lg bg-slate-700 text-white">Tambahkan</button>
      </form>
      </div>
      <ul>
  <li v-for="todo in filteredTodos" :key="todo.id">
    <input type="checkbox" v-model="todo.done">
    <span :class="{ done: todo.done }">{{ todo.text }}</span>
    <button @click="removeTodo(todo)" class="px-3">Hapus
      <TrashIcon class="h-6 w-6 text-gray-500" />
    </button>
  </li>
</ul>
<button class="border bg-slate-800 text-white text-base mt-3" @click="hideCompleted = !hideCompleted">
  {{ hideCompleted ? 'Show all' : 'Hide completed' }}
</button>
  </div>
</template>

<style scoped>
.container {
  max-width: 600px;
  margin: 0 auto;
  padding: 40px;
  background-color: #ffffff2f;
  border-radius: 20px;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.title {
  text-align: center;
  font-size: 28px;
  font-weight: bold;
  margin-bottom: 20px;
  color: #171717;
}

.form-container {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 20px;
}

.input-field {
  flex: 1;
  border: none;
  border-radius: 8px;
  padding: 12px;
  margin-right: 10px;
  font-size: 16px;
}

.add-button {
  border: none;
  border-radius: 8px;
  background-color: #4caf50;
  color: #ffffff;
  padding: 12px 24px;
  cursor: pointer;
  font-size: 16px;
}

.todos-container {
  list-style: none;
  padding: 0;
}

.todo-item {
  display: flex;
  align-items: center;
  margin-bottom: 16px;
}

.todo-text {
  flex: 1;
  margin-left: 16px;
  font-size: 18px;
  color: #333333;
}

.done {
  text-decoration: line-through;
}

.remove-button {
  background-color: #f44336;
  color: #ffffff;
  border: none;
  border-radius: 8px;
  padding: 8px 16px;
  cursor: pointer;
  font-size: 16px;
}

.remove-button:hover {
  background-color: #d32f2f;
}
</style>
