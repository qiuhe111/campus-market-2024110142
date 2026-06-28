<script setup lang="ts">
interface Feature {
  icon: string
  title: string
  desc: string
  link: string
}

const features: Feature[] = [
  { icon: '🔄', title: '二手交易', desc: '买卖闲置物品，教材、数码、生活用品一应俱全', link: '/trade' },
  { icon: '🔍', title: '失物招领', desc: '丢东西？捡到物品？来这里发布和查找', link: '/lost-found' },
  { icon: '🤝', title: '拼单搭子', desc: '外卖拼单、团购凑单，和同学一起省钱', link: '/group-buy' },
  { icon: '🏃', title: '跑腿委托', desc: '代取快递、代买午餐，轻松解决琐事', link: '/errand' },
]
</script>

<template>
  <div class="home">
    <section class="hero">
      <h1>校园集市</h1>
      <p class="subtitle">Campus Market —— 方便快捷的校园生活服务平台</p>
      <p class="desc">为在校师生提供二手交易、失物招领、拼单互助、跑腿委托等一站式服务</p>
      <router-link to="/trade" class="cta-btn">开始逛逛</router-link>
    </section>

    <section class="features">
      <h2>主要功能</h2>
      <div class="grid">
        <router-link
          v-for="item in features"
          :key="item.title"
          :to="item.link"
          class="feature-card"
        >
          <span class="icon">{{ item.icon }}</span>
          <h3>{{ item.title }}</h3>
          <p>{{ item.desc }}</p>
        </router-link>
      </div>
    </section>
  </div>
</template>

<style scoped>
.home {
  max-width: 960px;
  margin: 0 auto;
  padding: 48px 16px;
  min-height: 100vh;
  background: linear-gradient(180deg, #e8f4ff 0%, #ffffff 50%);
}

.hero {
  text-align: center;
  padding: 64px 0 48px;
}
.hero h1 {
  font-size: 40px;
  margin: 0 0 12px;
  color: #333;
}
.subtitle {
  font-size: 16px;
  color: #888;
  margin: 0 0 8px;
}
.desc {
  font-size: 15px;
  color: #666;
  margin: 0 0 32px;
  line-height: 1.6;
}
.cta-btn {
  display: inline-block;
  padding: 12px 40px;
  background: #409eff;
  color: #fff;
  border-radius: 8px;
  text-decoration: none;
  font-size: 16px;
  transition: background 0.2s;
}
.cta-btn:hover {
  background: #337ecc;
}

.features {
  padding: 32px 0;
}
.features h2 {
  text-align: center;
  font-size: 24px;
  margin: 0 0 32px;
  color: #333;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
  gap: 20px;
}

.feature-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 32px 20px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
  text-decoration: none;
  color: inherit;
  transition: transform 0.2s, box-shadow 0.2s;
}
.feature-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.1);
}

.icon {
  font-size: 48px;
  margin-bottom: 12px;
}
.feature-card h3 {
  margin: 0 0 8px;
  font-size: 18px;
  color: #333;
}
.feature-card p {
  margin: 0;
  font-size: 14px;
  color: #888;
  line-height: 1.5;
}
</style>
