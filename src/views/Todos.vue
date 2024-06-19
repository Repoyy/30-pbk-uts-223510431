<template>
  <div class="container">
    <h1 style="text-align: center;">Daftar Tugas</h1>
    <div style="display: flex;">
      <input type="text" v-model="newTask" placeholder="Tambahkan tugas baru...">
      <q-btn @click="addTask" label="Tambah" />
    </div>

    <div v-for="(task, index) in filteredTasks" :key="index" class="task" :class="{ 'completed': task.completed }">
      <q-checkbox v-model="task.completed" class="checkbox" @change="completeTask(index)">
        <div v-if="!task.editing" class="task-text">{{ task.title }}</div>
        <div v-else class="edit-mode">
          <q-input v-model="task.title" class="edit-input" dense />
          <div class="button-group">
            <q-btn @click="updateTask(index)" label="Perbarui" />
            <q-btn @click="cancelTask(index)" label="Batal" />
          </div>
        </div>
      </q-checkbox>
      <div class="button-group">
        <q-btn v-if="!task.editing" @click="editTask(index)" label="Edit" />
        <q-btn @click="deleteTask(index)" label="Hapus" />
      </div>
    </div>

    <q-btn class="filter-btn" @click="toggleShowIncomplete" :label="showIncomplete ? 'Tampilkan semua tugas' : 'Tampilkan hanya tugas yang belum selesai'" />
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const tasks = ref([]);

const newTask = ref('');
const showIncomplete = ref(true);

const addTask = () => {
  if (newTask.value.trim() !== '') {
    tasks.value.push({ title: newTask.value, completed: false, editing: false });
    newTask.value = '';
  }
};

const cancelTask = (index) => {
  tasks.value[index].editing = false;
};

const editTask = (index) => {
  tasks.value[index].editing = true;
};

const updateTask = (index) => {
  tasks.value[index].editing = false;
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

const completeTask = (index) => {
  tasks.value[index].completed = !tasks.value[index].completed;
};

const toggleShowIncomplete = () => {
  showIncomplete.value = !showIncomplete.value;
};

const filteredTasks = computed(() => {
  if (showIncomplete.value) {
    return tasks.value.filter(task => !task.completed);
  } else {
    return tasks.value;
  }
});

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

.edit-mode {
  display: flex;
  justify-content: space-between;
}

.edit-input {
  flex: 1;
  margin-right: 10px;
}

</style>
