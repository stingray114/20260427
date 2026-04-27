<script setup>
import { ref, onUnmounted } from 'vue'

const initialTime = ref(60)
const timeLeft = ref(60)
const timer = ref(null)

const startTimer = () => {
  if (timer.value) return
  timer.value = setInterval(() => {
    if (timeLeft.value > 0) timeLeft.value--
    else stopTimer()
  }, 1000)
}

const stopTimer = () => {
  clearInterval(timer.value)
  timer.value = null
}

const resetTimer = () => {
  stopTimer()
  timeLeft.value = initialTime.value
}

const updateTime = () => {
  if (initialTime.value < 0) initialTime.value = 0
  timeLeft.value = initialTime.value
}

onUnmounted(() => stopTimer())
</script>

<template>
  <div class="tool">
    <h2>⏱️ 時間管理</h2>
    <div class="config-area">
      <label>設定秒數：</label>
      <input 
        type="number" 
        v-model.number="initialTime" 
        @input="updateTime"
        :disabled="timer !== null"
      />
    </div>
    <div class="display">{{ timeLeft }} 秒</div>
    <div class="controls">
      <button @click="startTimer" :disabled="timer !== null || timeLeft <= 0">開始</button>
      <button @click="stopTimer">暫停</button>
      <button @click="resetTimer">重設</button>
    </div>
  </div>
</template>

<style scoped>
.tool {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  padding: 20px;
}
.config-area {
  text-align: center;
  margin-bottom: 10px;
}
.config-area input {
  width: 80px;
  padding: 5px;
  border-radius: 4px;
  border: 1px solid #dcdfe6;
  text-align: center;
  font-size: 1em;
}
.config-area input:disabled { background-color: #f5f7fa; cursor: not-allowed; }
.display {
  font-size: 3em;
  margin: 20px 0;
  text-align: center;
}
.controls {
  display: flex;
  gap: 10px;
  justify-content: center;
}
</style>