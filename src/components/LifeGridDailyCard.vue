<script setup lang="ts">
import { ref, computed } from 'vue'

// 设置生日（你可以修改为你自己）
const birthDate = new Date('1996-04-18')
const today = new Date()

// 获取今天是2025年的第几天（UTC精确）
const dayOfYear = computed(() => {
  const start = Date.UTC(today.getFullYear(), 0, 1)
  const now = Date.UTC(today.getFullYear(), today.getMonth(), today.getDate())
  const oneDay = 1000 * 60 * 60 * 24
  return Math.floor((now - start) / oneDay) + 1
})

// 判断是否为闰年
function isLeapYear(year: number) {
  return (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0
}

// 今年总天数
const daysInThisYear = computed(() => isLeapYear(today.getFullYear()) ? 366 : 365)

// 严格计算人生总天数
const lifeDay = computed(() => {
  const oneDay = 1000 * 60 * 60 * 24
  const utcBirth = Date.UTC(birthDate.getFullYear(), birthDate.getMonth(), birthDate.getDate())
  const utcNow = Date.UTC(today.getFullYear(), today.getMonth(), today.getDate())
  return Math.floor((utcNow - utcBirth) / oneDay) + 1
})

const totalLifeDays = 85 * 365  // 可后续考虑闰年更严谨处理

const todayAction = ref('写下第一句想说的话')
</script>

<template>
  <div class="card">
    <h2>{{ today.getFullYear() }}年 · 第{{ dayOfYear }}天</h2>
    <p class="title">今天我黑掉了这一格人生</p>

    <!-- 动态格子图：适配366天 -->
    <div class="grid">
      <div
        v-for="i in daysInThisYear"
        :key="i"
        :class="['cell', { passed: i < dayOfYear, today: i === dayOfYear }]"
      ></div>
    </div>

    <p class="action">📘 我做的事：{{ todayAction }}</p>
    <p class="life">这是我人生的第 {{ lifeDay }} 天 🕳️</p>
    <p class="ask">🖤 今天你做了什么？欢迎在评论区留言</p>
  </div>
</template>

<style scoped>
.card {
  background: #fff;
  padding: 20px;
  max-width: 420px;
  margin: 0 auto;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
  text-align: center;
}
.title {
  font-size: 20px;
  font-weight: bold;
  margin: 12px 0;
}
.grid {
  display: grid;
  grid-template-columns: repeat(21, 10px);
  gap: 2px;
  justify-content: center;
  margin: 20px 0;
}
.cell {
  width: 10px;
  height: 10px;
  background-color: #eee;
}
.cell.passed {
  background-color: #222;
}
.cell.today {
  background-color: red;
}
.action {
  font-size: 14px;
  margin-top: 10px;
  color: #333;
}
.life {
  font-size: 14px;
  color: #555;
}
.ask {
  font-weight: bold;
  font-size: 16px;
  margin-top: 12px;
}
</style>
