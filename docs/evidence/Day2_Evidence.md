# Day2 Evidence - 页面骨架与路由导航

## 1. 今日完成内容

今天完成了"校园轻集市"项目的页面骨架搭建和路由导航配置。首先重构了项目结构，将原有的零散页面文件整合到 `src/views/` 目录下，清理了不再使用的旧页面（ListView、DetailView、BoardView、ProfileView）。然后利用 Vue Router 定义了 8 个核心业务路由，并为每个路由绑定对应的页面组件。同时创建了可复用的公共布局组件体系，包括 AppLayout（整体布局容器）、AppHeader（头部品牌区）和 AppNav（导航菜单），使所有页面共享统一的视觉框架和导航能力。最后对首页进行了改版，设计为带 Hero 区域和四大功能卡片的介绍页，四个核心功能页面也分别补充了使用指南和温馨提示板块。

## 2. 页面与路由清单

| 页面名称 | 路由路径 | 文件位置 |
|---|---|---|
| 首页 | / | src/views/HomeView.vue |
| 二手交易 | /trade | src/views/TradeView.vue |
| 失物招领 | /lost-found | src/views/LostFoundView.vue |
| 拼单搭子 | /group-buy | src/views/GroupBuyView.vue |
| 跑腿委托 | /errand | src/views/ErrandView.vue |
| 发布信息 | /publish | src/views/PublishView.vue |
| 消息中心 | /message | src/views/MessageView.vue |
| 个人中心 | /user | src/views/UserCenterView.vue |

## 3. AI 协作记录

本次开发使用了 Opencode（AI CLI 编程助手）辅助完成以下内容：

- **AI 生成的部分**：
  1. 页面组件骨架代码：8 个视图文件的基础结构（`<template>` + `<script setup>` + `<style scoped>`）
  2. 公共布局组件：AppLayout、AppHeader、AppNav 的完整实现，包含 TypeScript 类型定义和 scoped 样式
  3. 路由配置文件：在 `src/router/index.ts` 中生成 8 条路由定义，包含 path、name、component 和 meta.title
  4. 首页改版：Hero 区域 + 功能卡片网格的布局和样式
  5. 四个功能详情页：TradeView、LostFoundView、GroupBuyView、ErrandView 的使用指南和温馨提示内容

- **核心提示词**：要求 AI 创建"校园轻集市"PC 端 Vue3 Web App 的 Day2 页面骨架与路由导航，指定技术栈为 Vue3 + Vite + TypeScript + Vue Router + Pinia，要求代码保持简洁、适合教学实训项目继续扩展。

- **人工检查与修改**：
  1. 发现 `TradeView.vue` 中的 action 按钮指向 `/list`，但该项目并未定义该路由路径，属于链接指向错误，已修改为 `/trade`
  2. 检查发现导航栏和首页中使用的"拼单互助"与路由元信息中的"拼单搭子"不一致，已全部统一为"拼单搭子"
  3. 确认所有 8 个页面的路由路径与组件名称一一对应，不存在遗漏或错配
  4. 确认所有导航项均使用 `<router-link>` 而非 `<a>` 标签，确保 SPA 跳转不触发页面刷新
  5. 确认 `main.ts` 已正确挂载 router 和 Pinia，`App.vue` 正确引入 AppLayout 布局组件

## 4. 遇到的问题与解决方法

**问题 1：AI 生成了不存在的路由链接**。`TradeView.vue` 的"去逛逛二手商品"按钮指向 `/list`，但本项目中仅定义了 8 个基础路由，`/list` 尚未创建。如果用户点击该按钮将跳转到空白页面。解决方法：人工审查每条路由链接，将该按钮的目标路径改为 `/trade`，使其指向本页面的合理目标。

**问题 2：导航栏名称与路由元信息不一致**。AI 生成 AppNav 时使用了"拼单互助"作为显示文本，但路由配置中 meta.title 写的是"拼单搭子"，首页的功能卡片也用了"拼单互助"。这会导致用户在导航栏和首页看到不同的功能名称，造成混淆。解决方法：统一将三处（AppNav、HomeView 功能卡片、证据文档）中的"拼单互助"改为"拼单搭子"，与路由元信息保持一致。

**问题 3：证据卡数据与实际代码不符**。AI 首次生成的证据卡中路由表存在多处错误，包括路径 `/user-center` 实际代码为 `/user`、路由 name `Trade` 实际为 `trade`、路由加载方式标注为"懒加载"但代码中使用的是直接导入。解决方法：逐项对照实际代码修正路由表和项目结构描述。

## 5. 今日反思

页面骨架是整个应用的地基，它决定了项目目录组织方式和代码扩展方向。通过今天的工作，我深刻体会到清晰的页面骨架能大幅降低后续开发的认知负担——每个业务模块有固定的文件位置，新增页面时只需在 views 目录下创建新文件并在路由表中注册即可。路由导航则是应用的"交通系统"，它不仅控制 URL 与页面的对应关系，还通过命名路由、路由元信息等机制为后续的权限控制和动态加载预留了扩展点。而公共布局组件将头部、导航、内容容器等跨页面共享的部分统一管理，避免了每个页面重复编写相同结构的模板代码。三者结合形成了一个可维护、可扩展的基础架构：页面骨架提供组织规范，路由导航提供访问控制，公共布局提供视觉一致性，这对后续接入真实业务数据、实现用户交互功能至关重要。
