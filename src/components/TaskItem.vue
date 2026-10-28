<script setup>
defineProps({
  task: {
    type: Object,
    required: true,
  },
})

const emit = defineEmits(['complete', 'delete'])
</script>

<template>
  <div
    class="task"
    :class="{ done: task.completed, high: task.priority === 'High' }"
  >
    <div class="task-info">
      <h3>{{ task.title }}</h3>
      <p>Priority: {{ task.priority }}</p>
      <p>Status: {{ task.completed ? 'Completed' : 'Active' }}</p>
    </div>

    <div class="task-buttons">
      <button
        v-if="!task.completed"
        class="btn btn-complete"
        @click="emit('complete', task.id)"
      >
        Complete
      </button>
      <button class="btn btn-delete" @click="emit('delete', task.id)">
        Delete
      </button>
    </div>
  </div>
</template>

<style>
.task {
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 12px;
  padding: 12px 16px;
  margin-bottom: 12px;
  border: 1px solid #e5e5e5;
  border-left: 4px solid #e5e5e5;
  border-radius: 6px;
}

.task.high {
  border-left-color: #d9534f;
}

.task.done h3 {
  text-decoration: line-through;
  color: #888;
}

.task-info h3 {
  margin: 0 0 6px 0;
  font-size: 15px;
}

.task-info p {
  margin: 2px 0;
  font-size: 12px;
  color: #666;
}

.task-buttons {
  display: flex;
  gap: 8px;
}

.btn {
  padding: 6px 12px;
  border: none;
  border-radius: 4px;
  color: white;
  font-size: 12px;
  cursor: pointer;
}

.btn-complete {
  background: #2e9e4f;
}

.btn-delete {
  background: #d9534f;
}
</style>