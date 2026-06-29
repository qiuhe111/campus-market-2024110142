<template>
  <section class="page">
    <div class="page-header">
      <h1>失物招领</h1>
      <p>丢失物品或捡到失物？来这里发布信息，互帮互助。</p>
    </div>

    <EmptyState
      v-if="lostFounds.length === 0"
      text="暂无失物招领信息"
    />

    <div class="list" v-else>
      <ItemCard
        v-for="item in lostFounds"
        :key="item.id"
        :title="item.title"
        :description="item.description"
        :tag="item.type === 'lost' ? '寻物' : '拾物'"
        :location="item.location"
        :time="item.eventTime"
      >
        <template #footer>
          <span>物品：{{ item.itemName }}</span>
        </template>
      </ItemCard>
    </div>
  </section>
</template>

<script setup lang="ts">
import { onMounted, ref } from 'vue'
import EmptyState from '../components/EmptyState.vue'
import ItemCard from '../components/ItemCard.vue'
import { getLostFounds, type LostFoundItem } from '../api/lostFound'

const lostFounds = ref<LostFoundItem[]>([])

onMounted(async () => {
  const res = await getLostFounds()
  lostFounds.value = res.data
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
</style>
