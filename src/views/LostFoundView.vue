<script setup lang="ts">
</script>

<template>
  <div class="feature-page">
    <router-link to="/" class="back-link">← 返回首页</router-link>

    <div class="content">
      <div class="header">
        <span class="icon">🔍</span>
        <h1>失物招领</h1>
        <p class="desc">丢失物品或捡到失物？来这里发布信息，互帮互助</p>
      </div>

      <section class="section">
        <h2>使用指南</h2>
        <ol>
          <li>发布失物：描述丢失物品的特征、时间、地点</li>
          <li>发布拾物：上传捡到物品的照片和发现地点</li>
          <li>搜索匹配：按类别和关键词搜索相关失物信息</li>
          <li>联系认领：通过平台预留方式联系失主或拾主</li>
        </ol>
      </section>

      <section class="section tips">
        <h2>温馨提示</h2>
        <ul>
          <li>发布时尽量提供详细的物品特征信息</li>
          <li>贵重物品建议附上唯一标识（如序列号）</li>
          <li>认领时请提供购买凭证或物品特征证明</li>
        </ul>
      </section>

      <router-link to="/message" class="action-btn">去失物招领区</router-link>
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
