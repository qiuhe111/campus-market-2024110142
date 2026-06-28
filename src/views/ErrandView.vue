<script setup lang="ts">
</script>

<template>
  <div class="feature-page">
    <router-link to="/" class="back-link">← 返回首页</router-link>

    <div class="content">
      <div class="header">
        <span class="icon">🏃</span>
        <h1>跑腿委托</h1>
        <p class="desc">代取快递、代买午餐、代办事宜，轻松解决日常琐事</p>
      </div>

      <section class="section">
        <h2>使用指南</h2>
        <ol>
          <li>发布任务：描述跑腿内容、地点和报酬</li>
          <li>接单响应：跑腿者查看任务列表并接单</li>
          <li>完成任务：跑腿者按要求完成任务并送达</li>
          <li>确认评价：委托方确认完成并评价服务</li>
        </ol>
      </section>

      <section class="section tips">
        <h2>温馨提示</h2>
        <ul>
          <li>发布任务时写清楚详细要求和时间限制</li>
          <li>跑腿费建议参考距离和任务难度合理定价</li>
          <li>贵重物品代取建议拍摄签收视频</li>
        </ul>
      </section>

      <router-link to="/publish" class="action-btn">去发布跑腿任务</router-link>
    </div>
  </div>
</template>

<style scoped>
.feature-page {
  max-width: 720px;
  margin: 0 auto;
  padding: 32px 16px;
  min-height: 100vh;
  background: linear-gradient(180deg, #e8f4ff 0%, #ffffff 50%);
}
.back-link {
  display: inline-block;
  margin-bottom: 24px;
  color: #409eff;
  text-decoration: none;
  font-size: 14px;
}
.back-link:hover { text-decoration: underline; }
.header { text-align: center; padding: 32px 0; }
.header .icon { font-size: 64px; display: block; margin-bottom: 16px; }
.header h1 { margin: 0 0 12px; font-size: 28px; color: #333; }
.header .desc { margin: 0; font-size: 16px; color: #888; }
.section {
  background: #fff;
  border-radius: 12px;
  padding: 24px;
  margin-bottom: 20px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}
.section h2 { margin: 0 0 16px; font-size: 18px; color: #333; }
.section ol, .section ul { margin: 0; padding-left: 20px; }
.section li { font-size: 15px; color: #555; line-height: 1.8; }
.tips { border-left: 4px solid #e6a23c; }
.action-btn {
  display: block;
  text-align: center;
  padding: 14px;
  background: #409eff;
  color: #fff;
  border-radius: 8px;
  text-decoration: none;
  font-size: 16px;
  transition: background 0.2s;
}
.action-btn:hover { background: #337ecc; }
</style>
