<script setup>
import { ref, computed } from 'vue'
import TaskItem from './components/TaskItem.vue'

const tasks = ref([
  { id: 1, title: 'Learn Vue basics', completed: true, priority: 'High' },
  { id: 2, title: 'Practice Vue directives', completed: false, priority: 'Medium' },
  { id: 3, title: 'Create To Do App', completed: false, priority: 'Low' },
])

const newTitle = ref('')
const newPriority = ref('Medium')

const statusFilter = ref('All')
const priorityFilter = ref('All')

const filteredTasks = computed(() => {
  return tasks.value.filter((task) => {
    const statusOk =
      statusFilter.value === 'All' ||
      (statusFilter.value === 'Completed' && task.completed) ||
      (statusFilter.value === 'Active' && !task.completed)

    const priorityOk =
      priorityFilter.value === 'All' || task.priority === priorityFilter.value

    return statusOk && priorityOk
  })
})

const addTask = () => {
  if (newTitle.value.trim() === '') {
    return
  }

  tasks.value.push({
    id: Date.now(),
    title: newTitle.value.trim(),
    completed: false,
    priority: newPriority.value,
  })

  newTitle.value = ''
}

const completeTask = (id) => {
  const task = tasks.value.find((item) => item.id === id)
  task.completed = true
}

const deleteTask = (id) => {
  tasks.value = tasks.value.filter((item) => item.id !== id)
}
</script>

<template>
  <div class="app">
    <h1>Todo App</h1>

    <div class="add-form">
      <input
        v-model="newTitle"
        type="text"
        placeholder="Add a new task"
        @keyup.enter="addTask"
      />

      <select v-model="newPriority">
        <option>High</option>
        <option>Medium</option>
        <option>Low</option>
      </select>

      <button @click="addTask">Add task</button>
    </div>

    <div class="filters">
      <select v-model="statusFilter">
        <option>All</option>
        <option>Active</option>
        <option>Completed</option>
      </select>

      <select v-model="priorityFilter">
        <option>All</option>
        <option>High</option>
        <option>Medium</option>
        <option>Low</option>
      </select>
    </div>

    <p v-if="filteredTasks.length === 0" class="empty">No tasks available.</p>

    <TaskItem
      v-for="task in filteredTasks"
      :key="task.id"
      :task="task"
      @complete="completeTask"
      @delete="deleteTask"
    />
  </div>
</template>

<style>
body {
  margin: 0;
  background: #f0f0f0;
  font-family: Arial, sans-serif;
}

.app {
  max-width: 600px;
  margin: 40px auto;
  padding: 24px;
  background: white;
  border-radius: 12px;
  box-shadow: 0 2px 10px rgba(0, 0, 0, 0.15);
}

.app h1 {
  margin: 0 0 20px 0;
  font-size: 22px;
}

.add-form {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  margin-bottom: 16px;
}

.add-form input {
  flex: 1;
  min-width: 160px;
  padding: 8px;
  border: 1px solid #333;
  border-radius: 6px;
}

.add-form select {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.add-form button {
  padding: 8px 16px;
  border: none;
  border-radius: 6px;
  background: #111;
  color: white;
  cursor: pointer;
}

.filters {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.filters select {
  padding: 6px;
  border: 1px solid #ccc;
  border-radius: 6px;
}

.empty {
  text-align: center;
  color: #666;
  padding: 20px 0;
}
</style>