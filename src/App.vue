<script setup lang="ts">
import { ref, computed } from 'vue'
const newTask = ref('')
const priority = ref('low')

const tasks = ref([
  { text: 'Learn vue', done: false, priority: 'Medium' },
  { text: 'Do homework', done: true, priority: 'High' }
])
const doneCount = computed(() => {
  return tasks.value.filter(task => task.done).length
})
function addTask() {
  if (newTask.value.trim() === "")
    return tasks.value.push({
      text: newTask.value,
      done: false,
      priority: priority.value
    })
  newTask.value = ''
  priority.value = 'low'

}
function removeTask(index) {
  tasks.value.splice(index, 1)
}

</script>

<template>
  <div class="app">
    <h1>Task Manager</h1>

    <p class="count">
      {{ doneCount }} of {{ tasks.length }} done
    </p>

    <div class="form">
      <input
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Enter task"
      />

      <select v-model="priority">
        <option>Low</option>
        <option>Medium</option>
        <option>High</option>
      </select>

      <button @click="addTask">Add</button>
    </div>

    <ul>
      <li v-for="(task, index) in tasks" :key="index">
        <input type="checkbox" v-model="task.done">

        <span :class="{ done: task.done }">
          {{ task.text }}
        </span>

        <span
          class="badge"
          :class="{
            low: task.priority === 'Low',
            medium: task.priority === 'Medium',
            high: task.priority === 'High'
          }"
        >
          {{ task.priority }}
        </span>

        <button @click="removeTask(index)">Delete</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.app {
  width: 500px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
}

.count {
  text-align: center;
  font-weight: bold;
}

.form {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input,
select,
button {
  padding: 8px;
}

input {
  flex: 1;
}

li {
  list-style: none;
  margin-bottom: 10px;
  display: flex;
  gap: 10px;
  align-items: center;
}

.done {
  text-decoration: line-through;
  color: gray;
}

.badge {
  padding: 4px 8px;
  border-radius: 5px;
  color: white;
  font-size: 12px;
}

.low {
  background: green;
}

.medium {
  background: orange;
}

.high {
  background: red;
}
</style>