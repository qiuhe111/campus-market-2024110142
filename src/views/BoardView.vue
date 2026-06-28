<script setup lang="ts"></script>

<template>
  <div class="board">
    <h1>看板页</h1>
    <div class="stats">
      <div class="card">
        <h3>商品总数</h3>
        <p class="num">128</p>
      </div>
      <div class="card">
        <h3>今日发布</h3>
        <p class="num">12</p>
      </div>
      <div class="card">
        <h3>注册用户</h3>
        <p class="num">356</p>
      </div>
      <div class="card">
        <h3>今日访问</h3>
        <p class="num">1,024</p>
      </div>
    </div>
  </div>
</template>

<style scoped>
.board {
  max-width: 800px;
  margin: 0 auto;
}
.stats {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(160px, 1fr));
  gap: 16px;
  margin-top: 24px;
}
.card {
  background: #f5f7fa;
  border-radius: 8px;
  padding: 24px;
  text-align: center;
}
.card h3 {
  margin: 0 0 8px;
  font-size: 14px;
  color: #666;
}
.card .num {
  margin: 0;
  font-size: 28px;
  font-weight: 700;
  color: #409eff;
}
</style>
