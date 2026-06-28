<script setup lang="ts">
const items = [
  { id: 1, title: '二手笔记本电脑', price: '¥3,200' },
  { id: 2, title: '高等数学教材', price: '¥25' },
  { id: 3, title: '山地自行车', price: '¥600' },
  { id: 4, title: '宿舍小冰箱', price: '¥400' },
]
</script>

<template>
  <div>
    <h1>列表页</h1>
    <ul class="item-list">
      <li v-for="item in items" :key="item.id">
        <router-link :to="`/detail/${item.id}`">
          <span class="title">{{ item.title }}</span>
          <span class="price">{{ item.price }}</span>
        </router-link>
      </li>
    </ul>
  </div>
</template>

<style scoped>
.item-list {
  list-style: none;
  padding: 0;
  max-width: 500px;
}
.item-list li {
  border-bottom: 1px solid #eee;
}
.item-list a {
  display: flex;
  justify-content: space-between;
  padding: 12px 0;
  text-decoration: none;
  color: #333;
}
.item-list a:hover {
  color: #409eff;
}
.price {
  color: #e6a23c;
  font-weight: 600;
}
</style>
