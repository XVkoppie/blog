<template>
  <div class="mini-calendar">
    <!-- 月份切换头部 -->
    <div class="calendar-header">
      <button class="nav-button" @click="prevMonth" title="上个月">◀</button>
      <h3>{{ currentYear }}年{{ currentMonth }}月</h3>
      <button class="nav-button" @click="nextMonth" title="下个月">▶</button>
    </div>

    <div class="calendar-weekdays">
      <div v-for="day in ['日', '一', '二', '三', '四', '五', '六']" :key="day" class="weekday">
        {{ day }}
      </div>
    </div>

    <div class="calendar-grid">
      <div
        v-for="(day, index) in calendarDays"
        :key="index"
        class="calendar-day"
        :class="{
          'other-month': !day.isCurrentMonth,
          today: day.isToday,
        }"
      >
        <div class="day-content">
          <div v-if="day.isCurrentMonth" class="day-number">{{ day.date.getDate() }}</div>
          <div
            v-if="day.isCurrentMonth"
            class="day-indicator"
            :style="{ backgroundColor: getDayColor(day.date) }"
            :title="getDayTitle(day.date)"
          ></div>
        </div>
      </div>
    </div>

    <!-- 图例 -->
    <div class="legend">
      <div class="legend-item" v-for="item in legendItems" :key="item.label">
        <div class="legend-color" :style="{ backgroundColor: item.color }"></div>
        <span>{{ item.label }}</span>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const currentDate = ref(new Date())

// 月份切换函数
const prevMonth = () => {
  const newDate = new Date(currentDate.value)
  newDate.setMonth(newDate.getMonth() - 1)
  currentDate.value = newDate
}

const nextMonth = () => {
  const newDate = new Date(currentDate.value)
  newDate.setMonth(newDate.getMonth() + 1)
  currentDate.value = newDate
}

// 计算属性
const currentYear = computed(() => currentDate.value.getFullYear())
const currentMonth = computed(() => currentDate.value.getMonth() + 1)

// 模拟打卡数据 - 实际应该从 API 获取
const focusData = ref({
  '2025-08-28': 2, // 8月数据
  '2025-08-15': 4,
  '2025-08-20': 6,

  '2025-09-01': 3, // 9月数据
  '2025-09-05': 1,
  '2025-09-10': 5,
  '2025-09-15': 2,
  '2025-09-20': 7,
  '2025-09-25': 4,
  '2025-09-28': 8,
  '2025-09-30': 6,

  '2025-10-05': 3, // 10月数据
  '2025-10-12': 5,
  '2025-10-20': 2,
})

// 图例项
const legendItems = ref([
  { color: '#ffffff', label: '0次' },
  { color: '#ffdeeb', label: '1-2次' },
  { color: '#fcc2d7', label: '3-4次' },
  { color: '#faa2c1', label: '5-6次' },
  { color: '#f783ac', label: '7+次' },
])

// 生成日历天数数组
const calendarDays = computed(() => {
  const year = currentDate.value.getFullYear()
  const month = currentDate.value.getMonth()
  const firstDay = new Date(year, month, 1)
  const lastDay = new Date(year, month + 1, 0)
  const firstDayOfWeek = firstDay.getDay() // 0是周日
  const daysInMonth = lastDay.getDate()

  const today = new Date()
  today.setHours(0, 0, 0, 0)

  const days = []

  // 填充上个月的空格
  const prevMonthLastDay = new Date(year, month, 0).getDate()
  for (let i = firstDayOfWeek - 1; i >= 0; i--) {
    const date = new Date(year, month - 1, prevMonthLastDay - i)
    days.push({
      date,
      isCurrentMonth: false,
      isToday: date.getTime() === today.getTime(),
    })
  }

  // 填充当前月的天数
  for (let i = 1; i <= daysInMonth; i++) {
    const date = new Date(year, month, i)
    date.setHours(0, 0, 0, 0)
    days.push({
      date,
      isCurrentMonth: true,
      isToday: date.getTime() === today.getTime(),
    })
  }

  // 填充下个月的空格（补齐6行42天）
  const totalCells = 42 // 6行 * 7天
  const remainingCells = totalCells - days.length
  for (let i = 1; i <= remainingCells; i++) {
    const date = new Date(year, month + 1, i)
    days.push({
      date,
      isCurrentMonth: false,
      isToday: date.getTime() === today.getTime(),
    })
  }

  return days
})

// 获取日期字符串（YYYY-MM-DD格式）
const getDateString = (date) => {
  const year = date.getFullYear()
  const month = String(date.getMonth() + 1).padStart(2, '0')
  const day = String(date.getDate()).padStart(2, '0')
  return `${year}-${month}-${day}`
}

// 获取某天的颜色
const getDayColor = (date) => {
  const dateStr = getDateString(date)
  const count = focusData.value[dateStr] || 0

  if (count === 0) return '#ffffff'
  if (count <= 2) return '#ffdeeb'
  if (count <= 4) return '#fcc2d7'
  if (count <= 6) return '#faa2c1'
  return '#f783ac'
}

// 获取某天的提示信息
const getDayTitle = (date) => {
  const dateStr = getDateString(date)
  const count = focusData.value[dateStr] || 0
  return `${dateStr}: ${count}次专注`
}
</script>

<style scoped>
.mini-calendar {
  max-width: 260px;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  background: white;
  border-radius: 12px;
  padding: 16px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}

.calendar-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 16px;
  padding: 0 4px;
}

.calendar-header h3 {
  margin: 0;
  font-size: 15px;
  font-weight: 600;
  color: #2d3748;
  text-align: center;
}

.nav-button {
  background: none;
  border: none;
  font-size: 14px;
  color: #4a5568;
  cursor: pointer;
  padding: 6px 10px;
  border-radius: 6px;
  transition: all 0.2s ease;
}

.nav-button:hover {
  background-color: #f7fafc;
  color: #2d3748;
  transform: scale(1.1);
}

.nav-button:active {
  transform: scale(0.95);
}

.calendar-weekdays {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 3px;
  margin-bottom: 8px;
}

.weekday {
  text-align: center;
  font-size: 11px;
  color: #718096;
  padding: 4px 0;
  font-weight: 500;
}

.calendar-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  gap: 3px;
}

.calendar-day {
  aspect-ratio: 1;
  min-height: 26px;
  position: relative;
}

.day-content {
  width: 100%;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.day-number {
  position: absolute;
  top: 3px;
  right: 3px;
  font-size: 9px;
  color: #4a5568;
  z-index: 2;
  font-weight: 500;
}

.day-indicator {
  width: 20px;
  height: 20px;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.2s ease;
  border: 1px solid rgba(0, 0, 0, 0.08);
  box-shadow: 0 1px 2px rgba(0, 0, 0, 0.05);
}

.day-indicator:hover {
  transform: scale(1.12);
  box-shadow: 0 0 0 2px #d6336c76;
  z-index: 3;
}

.other-month .day-indicator {
  opacity: 0.4;
}

.other-month .day-number {
  opacity: 0.5;
}

.today .day-number {
  color: #2a3f52;
  font-weight: 700;
}

.today .day-indicator {
  border: 2px solid #f06595;
}

/* 图例样式 */
.legend {
  display: flex;
  justify-content: space-between;
  margin-top: 16px;
  padding-top: 12px;
  border-top: 1px solid #e2e8f0;
}

.legend-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
}

.legend-color {
  width: 14px;
  height: 14px;
  border-radius: 3px;
  border: 1px solid rgba(0, 0, 0, 0.1);
}

.legend-item span {
  font-size: 9px;
  color: #718096;
  font-weight: 500;
}
</style>
