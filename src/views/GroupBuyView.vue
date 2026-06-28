<script setup lang="ts">
</script>

<template>
  <div class="feature-page">
    <router-link to="/" class="back-link">← 返回首页</router-link>

    <div class="content">
      <div class="header">
        <span class="icon">🤝</span>
        <h1>拼单互助</h1>
        <p class="desc">外卖拼单、团购凑单，和同学们一起省钱省力</p>
      </div>

      <section class="section">
        <h2>使用指南</h2>
        <ol>
          <li>发起拼单：发布拼单需求，标注商品和人数</li>
          <li>加入拼单：浏览进行中的拼单，申请加入</li>
          <li>在线支付：通过平台统一收款，由发起人下单</li>
          <li>确认收货：收到商品后确认完成</li>
        </ol>
      </section>

      <section class="section tips">
        <h2>温馨提示</h2>
        <ul>
          <li>发起拼单时请明确截止时间和取餐地点</li>
          <li>选择熟悉的同学一起拼单更可靠</li>
          <li>涉及金额较大时建议使用平台担保支付</li>
        </ul>
      </section>

      <router-link to="/message" class="action-btn">去看看拼单</router-link>
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
