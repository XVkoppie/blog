<!-- 创建时间：2025-09-16 09:17 -->
<!-- 功能：音乐播放器组件 -->
<!-- 文件名：musicePlayer.vue -->
<script setup lang="ts">
import { onMounted, reactive } from 'vue'

const state = reactive({
  dataCount: [],
  curYear: null,
  curMonth: null,
  curDate: null,
})
onMounted(() => {
  let date = new Date()
  state.curYear = date.getFullYear()
  state.curMonth = date.getMonth()
  state.curDate = date.getDate()

  getDateCount()
})

const getDateCount = () => {
  let count = new Date(state.curYear, state.curMonth + 1, 0).getDate()
  let firstday = new Date(state.curYear, state.curMonth, 1).getDay()
  for (let i = 1; i <= count + firstday; i++) {
    let val = i - firstday
    state.dataCount.push(val)
  }
}

const filterDate = (date) => {
  return date > 0 ? date : ''
}
</script>

<template>
  <div style="width: 400px; border: 1px solid #ccc; padding: 14px">
    <div
      style="
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: space-between;
        padding: 10px 0;
        border-bottom: 1px solid #ccc;
      "
    >
      <div class="left"><span>《</span><span><</span></div>
      <div class="title">当前时间</div>
      <div class="right"><span>></span><span>》</span></div>
    </div>

    <div style="width: 100%">
      <ul style="display: flex; justify-content: space-around; padding: 0">
        <li>日</li>
        <li>一</li>
        <li>二</li>
        <li>三</li>
        <li>四</li>
        <li>五</li>
        <li>六</li>
      </ul>
    </div>
    <div
      style="
        width: 100%;
        display: flex;
        flex-wrap: wrap;
        box-sizing: border-box;
      "
    >
      <template v-for="(item, index) in state.dataCount" :key="index">
        <div
          style="
            width: calc(100% / 7 - 16px);
            height: auto;
            text-align: center;
            padding: 8px 0;
            margin: 8px;
            box-sizing: border-box;
            border-radius: 50%;
          "
        >
          <span>{{ filterDate(item) }}</span>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped>
li {
  list-style: none;
}
</style>
