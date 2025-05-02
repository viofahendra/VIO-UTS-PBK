<script setup>
import { ref, computed, onMounted } from 'vue';

const tasks = ref([
  { name: "Belajar Vue", done: false },
  { name: "Mengerjakan tugas", done: true },
  { name: "Olahraga pagi", done: false },
  { name: "Membaca buku", done: true }
]);

const newTask = ref("");
const inputField = ref(null);

const isEditing = ref(false);
const editIndex = ref(null);
const showCompleted = ref(true);

const addTask = () => {
  const trimmed = newTask.value.trim();
  if (trimmed) {
    tasks.value.push({ name: trimmed, done: false });
    newTask.value = "";
    inputField.value?.focus();
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
  cancelEdit();
};

const editTask = (index) => {
  newTask.value = tasks.value[index].name;
  isEditing.value = true;
  editIndex.value = index;
  inputField.value?.focus();
};

const updateTask = () => {
  const trimmed = newTask.value.trim();
  if (editIndex.value !== null && trimmed) {
    tasks.value[editIndex.value].name = trimmed;
    cancelEdit();
  }
};

const cancelEdit = () => {
  newTask.value = "";
  isEditing.value = false;
  editIndex.value = null;
};

const filteredTasks = computed(() =>
  showCompleted.value
    ? tasks.value
    : tasks.value.filter(task => !task.done)
);

onMounted(() => {
  inputField.value?.focus();
});
</script>

<template>
 <div id="app">
    <div class="container">
      <div class="card">
        <h2>📋 Daftar Kegiatan</h2>

        <label class="toggle-show">
          <input type="checkbox" v-model="showCompleted" />
          Tampilkan yang sudah selesai
        </label>

        <ul class="task-list">
          <li v-for="(task, index) in filteredTasks" :key="index">
            <div class="task-content">
              <input type="checkbox" v-model="task.done" />
              <span :class="{ done: task.done }">{{ index + 1 }}. {{ task.name }}</span>
            </div>
            <div class="action-buttons">
              <button class="edit-btn" @click="editTask(index)">✏</button>
              <button class="delete-btn" @click="deleteTask(index)">🗑</button>
            </div>
          </li>
        </ul>

        <div class="input-group">
          <input
            ref="inputField"
            v-model="newTask"
            type="text"
            placeholder="Masukkan kegiatan baru"
          />
          <button v-if="isEditing" @click="updateTask">Update</button>
          <button v-else @click="addTask">Tambah</button>
          <button v-if="isEditing" class="cancel-btn" @click="cancelEdit">Batal</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style>
html,
body,
#app {
  margin: 0;
  padding: 0;
  width: 100%;
  min-height: 100vh;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  background-color: #f1f3f5;
  display: flex;
  justify-content: center;
  align-items: flex-start;
}

.container {
  flex: 1;
  display: flex;
  justify-content: center;
  padding: 40px 20px;
  box-sizing: border-box;
}

.card {
  width: 100%;
  max-width: 500px;
  background: #ffffff;
  border-radius: 16px;
  padding: 30px 20px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.1);
  display: flex;
  flex-direction: column;
}

h2 {
  color: #343a40;
  text-align: center;
  margin-bottom: 20px;
}

.toggle-show {
  display: flex;
  align-items: center;
  margin-bottom: 15px;
  font-size: 15px;
  color: #495057;
  gap: 8px;
}

.task-list {
  list-style: none;
  padding: 0;
  margin: 0;
  max-height: 300px;
  overflow-y: auto;
}

.task-list li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 0;
  border-bottom: 1px solid #dee2e6;
}

.task-content {
  display: flex;
  align-items: center;
  gap: 10px;
  flex-wrap: wrap;
}

.done {
  text-decoration: line-through;
  color: #868e96;
}

.action-buttons {
  display: flex;
  gap: 8px;
}

.edit-btn,
.delete-btn {
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
}

.edit-btn {
  color: #339af0;
}

.delete-btn {
  color: #ff6b6b;
}

.input-group {
  display: flex;
  gap: 10px;
  margin-top: 20px;
  flex-wrap: wrap;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  border: 1px solid #ced4da;
  border-radius: 6px;
  font-size: 15px;
  background-color: #f8f9fa;
}

button {
  padding: 10px 15px;
  font-size: 14px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.3s;
  background-color: #339af0;
  color: white;
}

button:hover {
  background-color: #228be6;
}

.cancel-btn {
  background-color: #fab005;
}
</style>