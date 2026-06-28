<script setup lang="ts">
interface NavItem {
  path: string
  label: string
}

const navItems: NavItem[] = [
  { path: '/', label: '首页' },
  { path: '/trade', label: '二手交易' },
  { path: '/lost-found', label: '失物招领' },
  { path: '/group-buy', label: '拼单搭子' },
  { path: '/errand', label: '跑腿委托' },
  { path: '/publish', label: '发布' },
  { path: '/message', label: '消息' },
  { path: '/user', label: '个人中心' },
]
</script>

<template>
  <nav class="app-nav">
    <router-link
      v-for="item in navItems"
      :key="item.path"
      :to="item.path"
      class="nav-link"
      active-class="active"
    >
      {{ item.label }}
    </router-link>
  </nav>
</template>

<style scoped>
.app-nav {
  display: flex;
  gap: 4px;
  padding: 8px 24px;
  background: #fff;
  border-bottom: 1px solid #f0f0f0;
}
.nav-link {
  padding: 6px 14px;
  font-size: 14px;
  color: #555;
  text-decoration: none;
  border-radius: 6px;
  transition: background 0.2s, color 0.2s;
}
.nav-link:hover {
  background: #e8f4ff;
  color: #409eff;
}
.nav-link.active {
  background: #409eff;
  color: #fff;
}
</style>
