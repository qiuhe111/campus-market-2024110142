# Day2 Evidence

- **日期**: 2026-06-28
- **主要任务**: 扩展 7 大页面骨架、完善路由系统、构建基础导航、进阶功能（看板统计 / 列表→详情跳转 / UI 优化）

---

## 今日完成内容

1. **页面骨架扩展（Task1）**
   - 在 `src/views/` 下新增 6 个页面：`ListView.vue`、`DetailView.vue`、`PublishView.vue`、`MessageView.vue`、`ProfileView.vue`、`BoardView.vue`
   - 每个页面包含 `<script setup lang="ts">`、`<template>`、`<style scoped>` 三部分，符合项目现有风格

2. **路由系统完善（Task2）**
   - 在 `src/router/index.ts` 中新增 7 条路由：`/home`、`/list`、`/detail/:id`、`/publish`、`/message`、`/profile`、`/board`
   - 保留原有 `/` 首页路由
   - 所有新增路由使用懒加载（`() => import(...)`）

3. **基础导航系统（Task3）**
   - 在 `App.vue` 中添加 `<nav>` 导航栏，包含 6 个入口：首页、列表、发布、消息、我的、看板
   - 使用 `<router-link>` 实现无刷新页面切换

4. **进阶任务**
   - **看板页设计**: 添加 4 个静态统计卡片（商品总数 128、今日发布 12、注册用户 356、今日访问 1,024）
   - **路由跳转增强**: 路由改为 `/detail/:id`，列表页展示 4 个模拟商品并跳转到详情页显示对应 ID，详情页包含返回链接
   - **UI 优化**: 检查发现项目未安装 Element Plus，使用原生 HTML + CSS 美化导航栏（蓝色主题、水平布局、hover 效果）

---

## 路由设计

| 路径 | 名称 | 页面组件 | 加载方式 |
|------|------|---------|---------|
| `/` | `home` | `HomeView` | 直接导入 |
| `/home` | `Home` | `HomeView.vue` | 懒加载 |
| `/list` | `List` | `ListView.vue` | 懒加载 |
| `/detail/:id` | `Detail` | `DetailView.vue` | 懒加载（带参数） |
| `/publish` | `Publish` | `PublishView.vue` | 懒加载 |
| `/message` | `Message` | `MessageView.vue` | 懒加载 |
| `/profile` | `Profile` | `ProfileView.vue` | 懒加载 |
| `/board` | `Board` | `BoardView.vue` | 懒加载 |

---

## 遇到的问题

1. **Dev server 进程管理**
   使用 Bash 工具启动 `npm run dev` 时，进程会因超时自动终止。先后尝试了 `Start-Process`、`Start-Job`、`cmd /c start` 等方式，最终通过直接运行（超时前已启动成功）和手动在浏览器访问解决。

2. **Element Plus 未安装**
   原本考虑使用 `<el-menu>` 优化导航栏，但检查 `package.json` 后发现项目并未安装 Element Plus，改为原生 HTML + CSS 实现，确保不引入外部依赖。

3. **Windows PowerShell 编码**
   部分错误信息和命令输出为乱码（中文环境下），但不影响功能正常运行。

---

## AI 协作记录

- **工具**: Opencode（AI CLI 编程助手）
- **协作次数**: 本次会话共 12 轮交互
- **协作内容**:
  1. 创建 6 个页面骨架文件（批量写入）
  2. 配置路由系统（编辑 `router/index.ts`）
  3. 构建导航系统（编辑 `App.vue`）
  4. 实现看板页静态统计信息
  5. 实现列表页 → 详情页带参数跳转
  6. 编写文档产出（Day2_Evidence.md）
- **协作模式**: 任务驱动——用户提出需求，AI 读取当前代码、理解上下文后执行修改，再通过 `vue-tsc --build` 验证

**使用感受**: AI 在批量创建模板文件、多文件并行编辑时效率很高。遇到 dev server 进程管理这类环境问题时，需要人工判断和配合。AI 能够根据已有代码风格自动保持一致性，减少手动调整的工作量。

---

## 自我评价

- **完成度**: 3 个主任务 + 3 个进阶任务全部完成
- **代码质量**: `vue-tsc --build` 编译通过，无类型错误
- **一致性**: 新增页面和路由严格遵循项目现有代码风格和约定
- **待加强**: 页面内容目前仍为骨架和静态数据，后续需要对接真实 API 和动态数据
