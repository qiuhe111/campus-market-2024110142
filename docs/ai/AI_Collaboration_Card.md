# AI Collaboration Card

## 基本信息

- **工具名称**: Opencode（AI CLI 编程助手）
- **使用日期**: 2026-06-27
- **任务描述**: 项目初始化理解 — 分析项目目录结构、核心代码和技术栈
- **使用场景**: 实训 Day1 — 项目背景与目录分析

---

## Prompt 1：分析项目目录结构

**Prompt**:
> 浏览项目目录，分析 campus-market-seed 的目录结构，解释每个目录的作用。

**AI 输出**:
```
campus-market-seed/
├── src/                # 主要源代码目录
│   ├── api/            # API 请求层，封装后端 HTTP 调用
│   ├── components/     # 公共可复用 UI 组件
│   ├── router/         # Vue Router 路由配置
│   ├── stores/         # Pinia 状态管理仓库
│   ├── views/          # 页面级视图组件
│   ├── App.vue         # 根组件（应用外壳 + 路由出口）
│   └── main.ts         # 应用入口，创建 Vue 实例
├── docs/               # 课程文档（指南/AI 规范/证据）
├── scripts/            # 自动检测工具（预留）
├── public/             # 静态资源
├── .editorconfig       # 编辑器统一配置
├── .nvmrc              # Node.js 版本锁定
├── vite.config.ts      # Vite 构建配置
├── package.json        # 依赖与脚本
└── tsconfig*.json      # TypeScript 配置
```

**自己的理解**:
采用关注点分离的分层架构，src 内按职责拆分为 5 个核心子目录（api/components/router/stores/views），符合 Vue 3 工程化最佳实践。基础设施配置集中在根目录，与业务代码分离。

**最终结论**:
目录结构清晰、约定明确，适合多人协作和 AI 辅助开发。

---

## Prompt 2：解释 main.ts

**Prompt**:
> 解释 src/main.ts 中每一行代码的作用。

**AI 输出**:
```ts
import { createApp } from 'vue'       // 从 Vue 导入创建应用的函数
import { createPinia } from 'pinia'   // 导入 Pinia 状态管理工厂函数
import App from './App.vue'           // 导入根组件
import router from './router'         // 导入路由配置

const app = createApp(App)            // 以 App.vue 为根创建 Vue 应用实例
app.use(createPinia())                // 通过插件机制注册 Pinia
app.use(router)                       // 通过插件机制注册 Vue Router
app.mount('#app')                     // 将应用挂载到 index.html 的 #app 元素上
```

**自己的理解**:
遵循"创建 → 注册插件 → 挂载"的三步流程。Pinia 和 Router 都通过 `app.use()` 以插件形式注册，这是 Vue 3 的标准扩展方式。

**最终结论**:
main.ts 是应用的JS入口，职责单一、流程清晰。

---

## Prompt 3：解释 Router 的作用

**Prompt**:
> 阅读 src/router/index.ts，解释 Vue Router 在项目中的作用以及首页为什么能显示。

**AI 输出**:
```ts
const router = createRouter({
  history: createWebHistory(),          // 使用 HTML5 History 模式
  routes: [
    {
      path: '/',                        // 匹配根路径
      name: 'home',
      component: HomeView,             // 映射到 HomeView 组件
    },
  ],
})
```
- **Router 的作用**: 管理 URL 路径与视图组件的映射关系，实现 SPA 无刷新导航
- **首页显示的原因**: 用户访问 `/` 路径 → Router 匹配到 `HomeView` 组件 → 渲染到 App.vue 的 `<RouterView />` 中

**自己的理解**:
Router 是 SPA 的"交通指挥员"，它让浏览器地址栏变化时不需要刷新页面就能切换内容。

**最终结论**:
Vue Router 通过路由表将 URL 与视图组件关联，<RouterView> 是动态渲染的出口。

---

## Prompt 4：分析当前项目采用的技术栈

**Prompt**:
> 根据 package.json 和项目配置，分析 campus-market-seed 使用的技术栈。

**AI 输出**:
| 类别 | 技术 | 用途 |
|------|------|------|
| 框架 | Vue 3 (^3.5) | UI 框架 |
| 语言 | TypeScript (~6.0) | 类型安全 |
| 构建 | Vite (^8.0) | 快速开发/打包 |
| 路由 | Vue Router (^5.1) | SPA 导航 |
| 状态管理 | Pinia (^3.0) | 全局状态 |
| 代码规范 | ESLint + Oxlint | 代码检查 |
| 类型检查 | vue-tsc | Vue SFC 类型校验 |
| 包管理 | pnpm (11.9) | 依赖管理 |
| Node.js | >=22.18 / >=24.12 | 运行环境 |

**自己的理解**:
技术栈选择了目前 Vue 3 生态最主流的方案：Vite + Pinia + Vue Router + TypeScript。Vue Router 5 和 Pinia 3 都是最新的主要版本。ESLint + Oxlint 双检查保障代码质量。

**最终结论**:
技术栈新、生态成熟、配置完整，适合作为教学项目的统一基线。

---

## 总体总结

通过本次 AI 协作：
1. 快速理解了项目目录结构和各目录职责；
2. 掌握了 main.ts 的应用启动流程；
3. 理解了 Vue Router 在 SPA 中的核心作用；
4. 理清了项目的完整技术栈。

AI 工具能够快速给出结构化、准确的分析，减少了手动查阅文档的时间。后续开发中可以继续利用 AI 进行代码生成、问题排查和知识检索。
