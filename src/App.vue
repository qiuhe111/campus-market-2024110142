<script setup lang="ts"></script>

<template>
  <main class="app">
    <nav>
      <router-link to="/home">首页</router-link>
      <router-link to="/list">列表</router-link>
      <router-link to="/publish">发布</router-link>
      <router-link to="/message">消息</router-link>
      <router-link to="/profile">我的</router-link>
      <router-link to="/board">看板</router-link>
    </nav>
    <h1>校园轻集市</h1>
    <p>AI 辅助前端工程实践种子项目</p>
    <RouterView />
  </main>
</template>

<style scoped>
.app {
  padding: 32px;
}
nav {
  display: flex;
  gap: 16px;
  margin-bottom: 24px;
}
nav a {
  text-decoration: none;
  color: #409eff;
  font-weight: 500;
}
nav a:hover {
  text-decoration: underline;
}
</style>