<script setup>
import { ref } from 'vue'

// 即時投票數據
const question = ref('今天的課程內容是否有助於你的理解？')
const options = ['非常滿意', '滿意', '普通', '需要加強']
const votes = ref([0, 0, 0, 0])

const vote = (index) => {
  votes.value[index]++
}

// 開放式問答數據
const responses = ref([])
const userInput = ref('')

const submitResponse = () => {
  if (userInput.value.trim()) {
    responses.value.push(userInput.value.trim())
    userInput.value = ''
  }
}
</script>

<template>
  <div class="tool">
    <div class="quiz-layout">
      <!-- 左側：開放式問答 -->
      <div class="panel open-ended">
        <h3>💬 開放式回饋</h3>
        <div class="response-list">
          <div v-for="(res, i) in responses" :key="i" class="response-item">
            {{ res }}
          </div>
          <div v-if="responses.length === 0" class="placeholder">尚無留言...</div>
        </div>
        <div class="input-group">
          <input 
            v-model="userInput" 
            @keyup.enter="submitResponse" 
            placeholder="輸入想法並按 Enter..." 
          />
          <button @click="submitResponse">發送</button>
        </div>
      </div>

      <!-- 右側：及時投票 -->
      <div class="panel multiple-choice">
        <h3>📊 即時投票</h3>
        <p class="question">{{ question }}</p>
        <div v-for="(opt, index) in options" :key="index" class="option">
          <button class="vote-btn" @click="vote(index)">{{ opt }}</button>
          <span class="vote-count"> 票數: {{ votes[index] }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.tool {
  padding: 20px;
  flex-grow: 1;
  display: flex;
  flex-direction: column;
}

.quiz-layout {
  display: flex;
  gap: 30px;
  flex-grow: 1;
}

.panel { flex: 1; display: flex; flex-direction: column; }
.open-ended { border-right: 1px solid #ebeef5; padding-right: 20px; }

.response-list {
  flex-grow: 1;
  background: #f8f9fb;
  border-radius: 8px;
  padding: 10px;
  margin-bottom: 10px;
  overflow-y: auto;
}

.response-item {
  background: #ffffff;
  padding: 8px 12px;
  border-radius: 6px;
  margin-bottom: 8px;
  font-size: 0.9em;
  border: 1px solid #e4e7ed;
  color: #333;
}

.input-group { display: flex; gap: 8px; }
.input-group input { flex: 1; padding: 8px; border-radius: 4px; border: 1px solid #dcdfe6; background: #ffffff; color: #333; }

.question { font-weight: bold; margin-bottom: 20px; color: #42b983; }
.option { margin: 15px 0; display: flex; align-items: center; }
.vote-btn { width: 100px; background-color: #42b983; color: white; border: none; padding: 8px; cursor: pointer; border-radius: 4px; }
.vote-count { margin-left: 15px; font-family: monospace; font-size: 1.1em; }
.placeholder { color: #666; text-align: center; margin-top: 20px; }
</style>