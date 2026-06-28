<script setup lang="ts">
</script>

<template>
  <div class="feature-page">
    <router-link to="/" class="back-link">← 返回首页</router-link>

    <div class="content">
      <div class="header">
        <span class="icon">🔄</span>
        <h1>二手交易</h1>
        <p class="desc">校园二手物品买卖平台，让闲置物品找到新主人</p>
      </div>

      <section class="section">
        <h2>使用指南</h2>
        <ol>
          <li>发布商品：点击"发布"按钮，上传照片、填写描述和价格</li>
          <li>浏览商品：在列表页按分类筛选，找到心仪商品</li>
          <li>联系卖家：通过商品详情页的联系方式与卖家沟通</li>
          <li>线下交易：在校内约定地点当面验货交易</li>
        </ol>
      </section>

      <section class="section tips">
        <h2>温馨提示</h2>
        <ul>
          <li>发布时请上传清晰实物照片，提高成交率</li>
          <li>交易时选择校内公共场所，注意人身和财物安全</li>
          <li>建议当面验货后再付款</li>
        </ul>
      </section>

      <router-link to="/trade" class="action-btn">去看看商品</router-link>
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
