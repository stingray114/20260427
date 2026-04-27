<script setup>
import { ref } from 'vue'

const teams = ref([
  { name: '第一組', score: 0 },
  { name: '第二組', score: 0 },
  { name: '第三組', score: 0 }
])

const newTeamName = ref('')

const addTeam = () => {
  const name = newTeamName.value.trim() || `第 ${teams.value.length + 1} 組`
  teams.value.push({ name, score: 0 })
  newTeamName.value = ''
}
</script>

<template>
  <div class="tool">
    <div class="scoreboard-header">
      <h2>📊 記分板</h2>
      <div class="add-controls">
        <input 
          v-model="newTeamName" 
          placeholder="輸入組名..." 
          @keyup.enter="addTeam"
        />
        <button @click="addTeam" class="add-btn">新增組別</button>
      </div>
    </div>

    <div v-for="team in teams" :key="team.name" class="team-row">
      <span>{{ team.name }}</span>
      <div class="score-controls">
        <button @click="team.score--">-</button>
        <span class="score">{{ team.score }}</span>
        <button @click="team.score++">+</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tool {
  flex-grow: 1;
  padding: 20px;
  color: #2c3e50;
}

.scoreboard-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 20px;
  flex-wrap: wrap;
  gap: 10px;
}

.add-controls {
  display: flex;
  gap: 8px;
}

.add-controls input {
  padding: 8px;
  border: 1px solid #dcdfe6;
  border-radius: 4px;
}

.add-btn {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 8px 15px;
  border-radius: 4px;
  cursor: pointer;
}

.team-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 10px;
  border-bottom: 1px solid #dbdbdb;
}
.score {
  display: inline-block;
  width: 50px;
  text-align: center;
  font-weight: bold;
  font-size: 1.2em;
}
</style>