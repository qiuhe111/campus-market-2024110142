<template>
  <section class="page">
    <div class="page-header">
      <h1>跑腿委托</h1>
      <p>代取快递、代买午餐、代办事宜，轻松解决日常琐事。</p>
    </div>

    <EmptyState
      v-if="errands.length === 0"
      text="暂无跑腿委托信息"
    />

    <div class="list" v-else>
      <ItemCard
        v-for="item in errands"
        :key="item.id"
        :title="item.title"
        :description="item.description"
        :tag="item.taskType"
        :location="item.from"
        :time="item.deadline"
      >
        <template #footer>
          <strong>￥{{ item.reward }}</strong>
          <span class="to">送到：{{ item.to }}</span>
        </template>
      </ItemCard>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import EmptyState from '../components/EmptyState.vue'
import ItemCard from '../components/ItemCard.vue'
import { getErrands, type ErrandItem } from '../api/errand'

const errands = ref<ErrandItem[]>([])

onMounted(async () => {
  const res = await getErrands()
  errands.value = res.data
})
</script>

<style scoped>
.page {
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.page-header {
  padding: 24px;
  border-radius: 16px;
  background: #fff;
}

.page-header h1 {
  margin: 0 0 8px;
}

.page-header p {
  margin: 0;
  color: #6b7280;
}

.list {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 16px;
}

.to {
  margin-left: 12px;
  color: #6b7280;
}
</style>
