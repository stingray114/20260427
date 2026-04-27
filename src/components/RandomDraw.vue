<script setup>
import { ref, computed } from 'vue'

const namesText = ref('學生1\n學生2\n學生3\n學生4\n學生5')
const result = ref('')
const isSpinning = ref(false)
const rotation = ref(0)

const colors = [
  '#FF6384', '#36A2EB', '#FFCE56', '#4BC0C0', '#9966FF', 
  '#FF9F40', '#8BC34A', '#E91E63', '#00BCD4', '#FF5722'
]

const nameList = computed(() => {
  return namesText.value.split('\n').filter(n => n.trim() !== '')
})

const wheelStyle = computed(() => {
  const list = nameList.value
  if (list.length === 0) return {}
  
  const segmentAngle = 360 / list.length
  const gradientParts = list.map((_, i) => {
    const color = colors[i % colors.length]
    return `${color} ${i * segmentAngle}deg ${(i + 1) * segmentAngle}deg`
  })
  
  return {
    background: `conic-gradient(${gradientParts.join(', ')})`,
    transform: `rotate(${rotation.value}deg)`,
    transition: isSpinning.value ? 'transform 4s cubic-bezier(0.15, 0, 0, 1)' : 'none'
  }
})

const draw = () => {
  const list = nameList.value
  if (list.length === 0) {
    alert('請輸入名單')
    return
  }
  if (isSpinning.value) return

  isSpinning.value = true
  result.value = ''
  
  const randomIndex = Math.floor(Math.random() * list.length)
  const segmentAngle = 360 / list.length
  
  // 計算旋轉角度：多轉 5 圈 + 目標偏移 (讓指針指到選中區塊的正中央)
  const extraRotations = 5 * 360 
  // 指針在正上方 (0度)，我們要把目標 segment 轉到 0 度位置
  const targetAngle = 360 - (randomIndex * segmentAngle + segmentAngle / 2)
  
  // 確保旋轉是持續增加的，並修正角度偏移
  const currentActualRotation = rotation.value % 360
  rotation.value += extraRotations + (targetAngle - currentActualRotation + 360) % 360

  setTimeout(() => {
    isSpinning.value = false
    result.value = list[randomIndex]
  }, 4000)
}
</script>

<template>
  <div class="tool">
    <h2>🎲 隨機抽籤轉盤</h2>
    
    <div class="draw-container">
      <div class="input-area">
        <textarea v-model="namesText" placeholder="每行輸入一個名字..." :disabled="isSpinning"></textarea>
        <button @click="draw" :disabled="isSpinning">開始旋轉</button>
      </div>

      <div class="wheel-wrapper" v-if="nameList.length > 0">
        <div class="pointer">▼</div>
        <div class="wheel" :style="wheelStyle">
          <div 
            v-for="(name, i) in nameList" 
            :key="i" 
            class="wheel-label"
            :style="{ 
              transform: `rotate(${(i * (360/nameList.length)) + (180/nameList.length)}deg) translateY(-85px)` 
            }"
          >
            {{ name }}
          </div>
        </div>
      </div>
    </div>

    <div v-if="result" class="result">
      恭喜：<strong>{{ result }}</strong>
    </div>
  </div>
</template>

<style scoped>
.tool {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  padding: 20px;
}
.draw-container { display: flex; gap: 30px; align-items: center; margin-top: 20px; flex-wrap: wrap; }
.input-area { flex: 1; min-width: 200px; }
textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 10px;
  background: #ffffff;
  color: #333;
  border: 1px solid #dcdfe6;
}
.wheel-wrapper { position: relative; width: 250px; height: 250px; flex-shrink: 0; margin: 0 auto; }
.pointer { position: absolute; top: -20px; left: 50%; transform: translateX(-50%); z-index: 10; font-size: 24px; color: #ff4444; }
.wheel { 
  width: 100%; height: 100%; border-radius: 50%; border: 5px solid #333; 
  position: relative; overflow: hidden; box-shadow: 0 4px 15px rgba(0,0,0,0.2);
}
.wheel-label {
  position: absolute; top: 50%; left: 50%;
  width: 80px; margin-left: -40px;
  text-align: center; color: #ffffff; font-weight: bold; font-size: 0.8em;
  text-shadow: 2px 2px 4px rgba(0,0,0,0.8);
  pointer-events: none;
}
.result {
  margin-top: 30px; text-align: center;
  font-size: 1.5em;
  color: #42b983;
}
</style>