<!-- 创建时间：2025-09-12 08:28 -->
<!-- 功能： -->
<!-- 文件名：index.vue -->
<script setup lang="ts"></script>

<template>
  <div class="bg">
    <div class="content-bg" style="flex: 8">
      <div>
        <focus />
      </div>
    </div>
    <div style="flex: 2; display: flex; flex-direction: column; gap: 20px">
      <div
        class="content-bg"
        style="
          flex: 1;
          display: flex;
          align-items: center;
          justify-content: space-around;
        "
      >
        <img
          style="width: 42px; cursor: pointer"
          src="../../assets/image/日记.png"
          alt=""
        />
        <img
          style="width: 42px; cursor: pointer"
          src="../../assets/image/光碟.png"
          alt=""
        />
        <img
          style="height: 42px; cursor: pointer"
          src="../../assets/image/文件.png"
          alt=""
        />
        <img
          style="height: 42px; cursor: pointer"
          src="../../assets/image/熊.png"
          alt=""
        />
      </div>
      <div class="content-bg" style="flex: 16">
        <heatMap />
      </div>
    </div>
  </div>
  <!-- <notepad /> -->
  <!-- <musicePlayer /> -->
  <!-- <calendar /> -->
</template>

<style scoped>
.bg {
  height: 100vh;
  width: 100vw;
  padding: 20px;
  background: url('@/assets/image/bg.jpg');
  background-position: center;
  background-size: cover;
  display: flex;
  gap: 20px;
}
.content-bg {
  padding: 15px;
  height: 100%;
  width: 100%;
  background-color: rgba(255, 255, 255, 0.921);
  border-radius: 12px;
}
</style>
