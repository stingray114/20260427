<script setup>
import { ref } from 'vue'
import RandomDraw from './components/RandomDraw.vue'
import Timer from './components/Timer.vue'
import Scoreboard from './components/Scoreboard.vue'
import Quiz from './components/Quiz.vue'
import Buzzer from './components/Buzzer.vue'

const currentTool = ref('draw')

const tools = [
  { id: 'draw', name: '抽籤機', icon: '🎲' },
  { id: 'timer', name: '計時器', icon: '⏱️' },
  { id: 'score', name: '記分板', icon: '📊' },
  { id: 'quiz', name: '及時問答', icon: '❓' },
  { id: 'buzzer', name: '搶答機', icon: '🔔' }
]
</script>

<template>
  <div class="app-shell">
    <div class="app-container">
    <header>
      <h1>教學輔助工具平台</h1>
      <nav>
        <button 
          v-for="tool in tools" 
          :key="tool.id"
          @click="currentTool = tool.id"
          :class="{ active: currentTool === tool.id }"
        >
          {{ tool.icon }} {{ tool.name }}
        </button>
      </nav>
    </header>

    <main class="tool-content">
      <RandomDraw v-if="currentTool === 'draw'" />
      <Timer v-if="currentTool === 'timer'" />
      <Scoreboard v-if="currentTool === 'score'" />
      <Quiz v-if="currentTool === 'quiz'" />
      <Buzzer v-if="currentTool === 'buzzer'" />
    </main>
    </div>
  </div>
</template>

<style scoped>
.app-shell {
  background-color: #f5f7fa;
  color: #2c3e50;
  min-height: 100vh;
  font-family: Arial, sans-serif;
}
.app-container {
  width: 100%;
  height: 100vh;
  margin: 0;
  padding: 0;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  overflow: hidden;
}
header { padding-top: 20px; }
header h1 { color: #42b983; text-align: center; margin: 0 0 20px 0; }
nav { display: flex; gap: 10px; margin-bottom: 20px; justify-content: center; flex-wrap: wrap; padding: 0 10px; }

button {
  padding: 10px 20px;
  background: #ffffff;
  border: 1px solid #dcdfe6;
  color: #606266;
  cursor: pointer;
}
button.active {
  background-color: #42b983;
  color: white;
  border-color: #42b983;
}
.tool-content {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  width: 100%;
}
</style>
