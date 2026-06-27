# 校园轻集市 — 项目规划

## 一、页面清单

| 序号 | 页面 | 路由路径 | 说明 |
|------|------|---------|------|
| 1 | 首页（商品列表） | `/` | 展示全部商品，支持搜索和分类筛选 |
| 2 | 用户注册 | `/register` | 新用户注册表单 |
| 3 | 用户登录 | `/login` | 用户登录表单 |
| 4 | 商品详情 | `/product/:id` | 查看商品详细信息 |
| 5 | 发布商品 | `/product/create` | 新建商品发布 |
| 6 | 编辑商品 | `/product/:id/edit` | 编辑已发布的商品 |
| 7 | 购物车 | `/cart` | 购物车商品管理 |
| 8 | 订单列表 | `/orders` | 我的订单列表 |
| 9 | 订单详情 | `/order/:id` | 单个订单详情 |
| 10 | 个人中心 | `/profile` | 用户信息、我的发布、设置 |
| 11 | 商品分类 | `/category/:name` | 按分类浏览商品 |
| 12 | 搜索结果 | `/search` | 搜索结果展示页 |

---

## 二、功能模块

```
校园轻集市
├── 用户模块
│   ├── 注册
│   ├── 登录 / 登出
│   └── 个人信息管理
├── 商品模块
│   ├── 商品列表（首页）
│   ├── 商品搜索
│   ├── 商品分类浏览
│   ├── 商品详情
│   ├── 发布商品
│   ├── 编辑 / 下架商品
│   └── 我的发布
├── 购物车模块
│   ├── 加入购物车
│   ├── 修改数量
│   ├── 删除商品
│   └── 选中结算
├── 订单模块
│   ├── 创建订单
│   ├── 订单列表
│   ├── 订单详情
│   └── 取消订单
└── 公共模块
    ├── 导航栏（路由跳转 + 用户状态）
    ├── 页面底部
    ├── 加载状态 / 空状态 / 错误处理
    └── 全局样式与布局
```

---

## 三、开发顺序（建议按天分配）

```
Day1 ─── 项目初始化 + 路由配置 + 布局骨架
  │
Day2 ─── 用户模块（注册 / 登录 / 状态管理）
  │
Day3 ─── 商品模块（列表 / 详情 / 发布）
  │
Day4 ─── 搜索 + 分类 + 商品编辑 / 下架
  │
Day5 ─── 购物车模块（增删改查 + 状态同步）
  │
Day6 ─── 订单模块（下单 / 列表 / 详情）
  │
Day7 ─── 个人中心 + 整体联调 + 过程证据 + 验收
```

### 按依赖关系解释

```
路由配置 ──→ 布局骨架
   │
   ├──→ 用户模块（登录后才能操作）
   │        │
   │        ├──→ 发布商品（需要用户身份）
   │        ├──→ 购物车（需要用户身份）
   │        └──→ 订单（需要用户身份）
   │
   └──→ 商品列表（无需登录可浏览）
            │
            ├──→ 商品详情
            ├──→ 搜索 / 分类
            └──→ 商品管理（需要用户身份）
```

---

## 四、开发重点

### 4.1 重点一：路由架构与导航守卫（Day1）

- 配置完整路由表
- 实现路由懒加载（`() => import('@/views/XXX.vue')`）
- **路由守卫**：未登录用户访问 `/cart` `/orders` `/profile` 时自动跳转登录页

### 4.2 重点二：用户状态管理（Pinia）（Day2）

- `userStore`：管理 token、用户信息、登录/登出动作
- 登录成功后 token 存入 `localStorage`，页面刷新后恢复状态
- API 请求拦截器统一携带 token

### 4.3 重点三：API 封装层（Day2~3）

- 统一 `request` 函数（基于 `fetch` 或 `axios`）
- 统一错误处理、loading 状态
- 每个模块独立 API 文件（`api/user.ts`、`api/product.ts`、`api/order.ts`）

### 4.4 重点四：组件化设计（Day3~5）

- **通用组件**：`NavBar.vue`、`ProductCard.vue`、`LoadingSpinner.vue`、`EmptyState.vue`
- 组件遵循 **单向数据流**，通过 props 接收数据，emit 抛出事件

### 4.5 重点五：购物车状态同步（Day5）

- 购物车数据存放在 Pinia `cartStore`
- 数量修改、删除、选中结算等操作 **全部在 store 内完成**
- 组件只管渲染和派发 action

### 4.6 重点六：整体联调与边界处理（Day6~7）

- 空数据、网络错误、加载中三种状态的完整覆盖
- 用户无权限时的引导提示
- 表单验证（发布商品、注册）

---

## 五、技术实现要点

```ts
// 路由懒加载示例
const routes = [
  {
    path: '/login',
    name: 'login',
    component: () => import('@/views/LoginView.vue'),
  },
]

// 路由守卫示例
router.beforeEach((to, from, next) => {
  const userStore = useUserStore()
  const requiresAuth = to.meta.requiresAuth
  if (requiresAuth && !userStore.isLoggedIn) {
    next({ name: 'login', query: { redirect: to.fullPath } })
  } else {
    next()
  }
})
```

```ts
// API 封装核心思路
async function request<T>(url: string, options?: RequestInit): Promise<T> {
  const token = localStorage.getItem('token')
  const res = await fetch(url, {
    ...options,
    headers: {
      'Content-Type': 'application/json',
      ...(token ? { Authorization: `Bearer ${token}` } : {}),
      ...options?.headers,
    },
  })
  if (!res.ok) throw new Error(`HTTP ${res.status}`)
  return res.json()
}
```

---

## 六、项目规划总览图

```
校园轻集市 项目规划
│
├── 页面（12个）
│   ├── 首页/商品列表  ├── 注册      ├── 购物车
│   ├── 商品详情        ├── 登录      ├── 订单列表
│   ├── 发布商品        ├── 个人中心  ├── 订单详情
│   └── 编辑商品        └── 分类浏览  └── 搜索结果
│
├── 模块（5个）
│   ├── 用户     → 注册 / 登录 / 个人信息
│   ├── 商品     → 列表 / 详情 / 发布 / 搜索
│   ├── 购物车   → 增删改查 / 选中结算
│   ├── 订单     → 下单 / 列表 / 取消
│   └── 公共     → 导航 / 布局 / 加载 / 错误
│
├── 顺序（7天）
│   Day1: 路由 + 布局
│   Day2: 用户模块
│   Day3: 商品模块（基础）
│   Day4: 搜索 + 分类 + 管理
│   Day5: 购物车
│   Day6: 订单
│   Day7: 个人中心 + 联调 + 验收
│
└── 重点（6项）
    路由守卫 → 用户状态 → API封装 → 组件化 → 购物车同步 → 边界处理
```
