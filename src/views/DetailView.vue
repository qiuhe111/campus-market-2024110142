<script setup lang="ts">
import { ref, computed } from 'vue'
import { useRoute } from 'vue-router'

interface Product {
  id: number
  title: string
  price: string
  originalPrice?: string
  description: string
  category: string
  location: string
  contact: string
  publishDate: string
  status: '在售' | '已售出'
}

const allProducts: Product[] = [
  {
    id: 1,
    title: '二手笔记本电脑',
    price: '¥3,200',
    originalPrice: '¥6,999',
    description: '联想 ThinkPad X1 Carbon 第8代，i5处理器，16GB内存，512GB固态硬盘。2023年购入，屏幕无划痕，键盘完好。因换新机出售，适合办公和学习使用。',
    category: '数码产品',
    location: '北区宿舍3号楼',
    contact: '138****5678',
    publishDate: '2026-06-20',
    status: '在售',
  },
  {
    id: 2,
    title: '高等数学教材',
    price: '¥25',
    description: '同济大学第七版《高等数学》上下册，九成新，内有少量笔记。期末考完低价转让，适合大一新生。',
    category: '教材教辅',
    location: '图书馆自习室',
    contact: '189****2345',
    publishDate: '2026-06-18',
    status: '在售',
  },
  {
    id: 3,
    title: '山地自行车',
    price: '¥600',
    originalPrice: '¥1,200',
    description: '美利达勇士300，26寸轮径，24速变速。去年购买，骑行里程约500公里，车况良好。暑假离校需处理，附赠车锁和车灯。',
    category: '交通工具',
    location: '南区停车场B区',
    contact: '156****7890',
    publishDate: '2026-06-15',
    status: '在售',
  },
  {
    id: 4,
    title: '宿舍小冰箱',
    price: '¥400',
    originalPrice: '¥899',
    description: '海尔小型冰箱，容量50L，适合宿舍使用。制冷效果好，噪音小，外观干净无损伤。毕业季清仓出售。',
    category: '家用电器',
    location: '西区公寓2号楼',
    contact: '177****4321',
    publishDate: '2026-06-22',
    status: '已售出',
  },
]

const route = useRoute()
const id = Number(route.params.id)

const product = computed(() => allProducts.find((p) => p.id === id) ?? null)
const loading = ref(false)
</script>

<template>
  <div class="detail-page">
    <router-link to="/list" class="back-link">← 返回列表</router-link>

    <div v-if="loading" class="loading">加载中...</div>

    <div v-else-if="!product" class="not-found">
      <h2>商品不存在</h2>
      <p>未找到 ID 为 {{ id }} 的商品</p>
    </div>

    <div v-else class="detail-card">
      <div class="image-placeholder">
        <span class="image-icon">📷</span>
        <span class="image-label">{{ product.title }}</span>
      </div>

      <div class="info">
        <div class="header">
          <h1>{{ product.title }}</h1>
          <span :class="['status-tag', product.status === '在售' ? 'selling' : 'sold']">
            {{ product.status }}
          </span>
        </div>

        <div class="price-row">
          <span class="price">{{ product.price }}</span>
          <span v-if="product.originalPrice" class="original-price">
            原价 {{ product.originalPrice }}
          </span>
        </div>

        <p class="description">{{ product.description }}</p>

        <ul class="meta">
          <li><strong>分类：</strong>{{ product.category }}</li>
          <li><strong>交易地点：</strong>{{ product.location }}</li>
          <li><strong>联系方式：</strong>{{ product.contact }}</li>
          <li><strong>发布时间：</strong>{{ product.publishDate }}</li>
        </ul>

        <button class="contact-btn" :disabled="product.status === '已售出'">
          {{ product.status === '已售出' ? '已售出' : '联系卖家' }}
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.detail-page {
  max-width: 800px;
  margin: 0 auto;
  padding: 24px 16px;
}

.back-link {
  display: inline-block;
  margin-bottom: 20px;
  color: #409eff;
  text-decoration: none;
  font-size: 14px;
}
.back-link:hover {
  text-decoration: underline;
}

.loading,
.not-found {
  text-align: center;
  padding: 60px 0;
  color: #999;
}
.not-found h2 {
  margin-bottom: 8px;
  color: #333;
}

.detail-card {
  display: flex;
  gap: 32px;
  background: #fff;
  border-radius: 12px;
  padding: 24px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

.image-placeholder {
  flex-shrink: 0;
  width: 320px;
  height: 320px;
  background: #f5f7fa;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 8px;
  color: #aaa;
  font-size: 14px;
}
.image-icon {
  font-size: 48px;
}

.info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 16px;
}

.header {
  display: flex;
  align-items: flex-start;
  justify-content: space-between;
  gap: 12px;
}
.header h1 {
  margin: 0;
  font-size: 22px;
  color: #333;
}

.status-tag {
  flex-shrink: 0;
  padding: 2px 10px;
  border-radius: 4px;
  font-size: 12px;
  font-weight: 600;
}
.status-tag.selling {
  background: #e6f7e6;
  color: #67c23a;
}
.status-tag.sold {
  background: #fef0f0;
  color: #f56c6c;
}

.price-row {
  display: flex;
  align-items: baseline;
  gap: 12px;
}
.price {
  font-size: 28px;
  font-weight: 700;
  color: #e6a23c;
}
.original-price {
  font-size: 14px;
  color: #aaa;
  text-decoration: line-through;
}

.description {
  margin: 0;
  font-size: 15px;
  line-height: 1.7;
  color: #555;
}

.meta {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 8px;
  font-size: 14px;
  color: #666;
}
.meta strong {
  color: #333;
}

.contact-btn {
  align-self: flex-start;
  padding: 10px 28px;
  background: #409eff;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
  transition: background 0.2s;
}
.contact-btn:hover:not(:disabled) {
  background: #337ecc;
}
.contact-btn:disabled {
  background: #ccc;
  cursor: not-allowed;
}

@media (max-width: 680px) {
  .detail-card {
    flex-direction: column;
  }
  .image-placeholder {
    width: 100%;
    height: 200px;
  }
}
</style>
