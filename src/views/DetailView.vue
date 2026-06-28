<script setup lang="ts">
import { useRoute } from 'vue-router'

const route = useRoute()
const id = route.params.id
</script>

<template>
  <div>
    <h1>详情页</h1>
    <p>商品 ID：{{ id }}</p>
    <router-link to="/list">← 返回列表</router-link>
  </div>
</template>

<style scoped></style>
