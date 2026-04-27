<script setup>
import { ref } from 'vue'

const status = ref('idle') // idle, counting, active
const countdown = ref(3)
const buzzedList = ref([])
const teams = ['第一組', '第二組', '第三組', '第四組']

const startBuzzer = () => {
  buzzedList.value = []
  status.value = 'counting'
  countdown.value = 3
  
  const timer = setInterval(() => {
    countdown.value--
    if (countdown.value <= 0) {
      clearInterval(timer)
      status.value = 'active'
    }
  }, 1000)
}

const handleBuzz = (team) => {
  if (status.value !== 'active') return
  if (buzzedList.value.includes(team)) return
  
  buzzedList.value.push(team)
}

const reset = () => {
  status.value = 'idle'
  buzzedList.value = []
}
</script>

<template>
  <div class="tool">
    <div class="buzzer-header">
      <h2>🔔 搶答競賽</h2>
      <button v-if="status === 'idle'" @click="startBuzzer" class="action-btn start">開始</button>
      <button v-else @click="reset" class="action-btn reset">重置</button>
    </div>

    <!-- 核心互動區：倒數或搶答按鈕 -->
    <div class="main-stage">
      <div v-if="status === 'counting'" class="countdown-overlay">{{ countdown }}</div>
      
      <div class="buzzer-grid">
        <div 
          v-for="team in teams" 
          :key="team" 
          @click="handleBuzz(team)"
          :class="['buzzer-btn', { 
            buzzed: buzzedList.includes(team), 
            disabled: status !== 'active' && !buzzedList.includes(team) 
          }]"
        >
          <div class="team-name">{{ team }}</div>
          <div v-if="buzzedList.includes(team)" class="rank-badge">
            NO.{{ buzzedList.indexOf(team) + 1 }}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tool {
  padding: 20px;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}

.buzzer-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}

.action-btn {
  padding: 10px 25px;
  border: none;
  border-radius: 6px;
  font-weight: bold;
  cursor: pointer;
}
.start { background: #42b983; color: white; }
.reset { background: #909399; color: white; }

.main-stage {
  position: relative;
  flex-grow: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.countdown-overlay {
  position: absolute;
  font-size: 8em;
  font-weight: bold;
  color: #ffce56;
  z-index: 10;
  text-shadow: 0 0 20px rgba(0,0,0,0.5);
}

.buzzer-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 20px;
  width: 100%;
}

.buzzer-btn {
  background: #ffffff;
  height: 140px;
  border-radius: 12px;
  border: 2px solid #ebeef5;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: all 0.2s;
  position: relative;
  box-shadow: 0 2px 12px 0 rgba(0,0,0,0.05);
}

.buzzer-btn.disabled { opacity: 0.3; cursor: not-allowed; }
.buzzer-btn.buzzed { background: #42b983; border-color: #fff; }
.buzzer-btn:active:not(.disabled) { transform: scale(0.95); }

.team-name { font-size: 1.5em; font-weight: bold; }
.rank-badge {
  position: absolute;
  top: 10px;
  right: 10px;
  background: #ffce56;
  color: #000;
  padding: 2px 8px;
  border-radius: 4px;
  font-size: 0.8em;
  font-weight: bold;
}

h2 { color: #42b983; margin: 0; }
</style>