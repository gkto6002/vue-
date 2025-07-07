<script setup lang="ts">
import { ref, computed } from 'vue'

// タスクの型定義
interface Task {
  id: number
  name: string
  completed: boolean
}

// 状態
const tasks = ref<Task[]>([])
const newTaskName = ref('')

// タスク追加関数
const addTask = () => {
  const name = newTaskName.value.trim()
  if (!name) return

  tasks.value.push({
    id: Date.now(), // 一意なID（簡易版）
    name,
    completed: false,
  })

  newTaskName.value = ''
}

// タスクを完了にする関数
const toggleTask = (task: Task) => {
  task.completed = !task.completed
}

// 未完・完了タスクのフィルタ
const incompleteTasks = computed(() => tasks.value.filter(t => !t.completed))
const completedTasks = computed(() => tasks.value.filter(t => t.completed))
</script>

<template>
  <div>
    <h1> Todoリスト</h1>

    <!-- 新規タスク追加 -->
    <div>
      <input
        v-model="newTaskName"
        placeholder="新しいタスクを入力"
        @keyup.enter="addTask"
      />
      <button @click="addTask" :disabled="newTaskName.trim() === ''">追加</button>
    </div>

    <!-- 未完了タスク -->
    <h2> 未完了のタスク</h2>
    <ul>
      <li v-for="task in incompleteTasks" :key="task.id">
        {{ task.name }}
        <button @click="toggleTask(task)">完了にする</button>
      </li>
    </ul>

    <!-- 完了済みタスク -->
    <h2> 完了したタスク</h2>
    <ul>
      <li v-for="task in completedTasks" :key="task.id">
        <s>{{ task.name }}</s>
        <button @click="toggleTask(task)">未完に戻す</button>
      </li>
    </ul>
  </div>
</template>

<style scoped>
input {
  margin-right: 8px;
  padding: 4px;
}
button {
  margin-left: 8px;
}
s {
  color: gray;
}
</style>
