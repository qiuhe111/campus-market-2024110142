# Day5 Evidence - 状态管理与用户中心

## 1. 今日完成内容

今天完成了 Pinia 状态管理在校园轻集市中的落地实践。创建了两个核心 Store：`userStore` 和 `favoriteStore`，分别管理当前用户信息和收藏状态。更新了导航栏、发布页面、二手交易列表页和个人中心页面，使它们共享来自 Store 的响应式数据。用户中心实现了用户资料卡片、我的收藏展示和我的发布占位面板，消息中心也补充了基础页面结构。

## 2. Store 设计说明

| Store 文件 | 管理内容 | 主要状态 | 主要方法 |
|---|---|---|---|
| src/stores/user.ts | 当前用户信息 | isLoggedIn、currentUser | updateProfile、login、logout |
| src/stores/favorite.ts | 收藏状态 | favorites | addFavorite、removeFavorite、toggleFavorite、isFavorite |

userStore 使用 `defineStore('user', ...)` 定义，包含 displayName 和 userDescription 两个 getter，用于在导航栏和个人中心直接渲染格式化文本。favoriteStore 使用 `defineStore('favorite', ...)` 定义，提供了完整的增删查切换操作，所有收藏数据仅保存在前端内存中。

## 3. 状态边界说明

以下是数据是否放入 Store 的设计决策及原因：

- **当前用户信息放入 Store**：因为导航栏（AppHeader.vue）、发布页面（PublishView.vue）和个人中心（UserCenterView.vue）都需要读取同一份用户资料，放入 Store 可以避免 prop 逐层传递。
- **收藏列表放入 Store**：因为二手交易列表页（TradeView.vue）需要调用 `isFavorite` 和 `toggleFavorite`，个人中心需要展示完整收藏列表并支持取消收藏。跨页面共享的需求决定了它必须放在 Store 中。
- **表单校验错误不放入 Store**：校验错误只在发布页面本地使用，没有其他组件需要读取或修改，使用 reactive 局部管理更简洁。
- **发布表单临时输入不放入 Store**：`form` 对象是发布页面独有的临时数据，不需要与任何其他页面共享，放在组件内通过 reactive 管理即可。
- **消息列表数据不放入 Store**：当前消息中心只是静态展示两条示例消息，没有跨页面共享或状态变更需求，直接在模板中编写即可。

## 4. 页面使用记录

- **AppHeader.vue**：引入 `useUserStore`，使用 `userStore.displayName` 在导航栏右侧显示当前用户名。
- **PublishView.vue**：引入 `useUserStore`，将 publisher 字段从硬编码的 `'当前用户'` 改为 `userStore.displayName`，覆盖二手交易、拼单搭子和跑腿委托三类发布。
- **TradeView.vue**：引入 `useFavoriteStore`，在每个 ItemCard 的 footer 插槽中添加收藏按钮，调用 `toggleFavorite` 切换状态，按钮文字根据 `isFavorite` 动态显示"收藏"或"已收藏"。
- **UserCenterView.vue**：同时引入 `useUserStore` 和 `useFavoriteStore`，展示用户头像首字、昵称、学院年级、个人简介，以及收藏列表（每条支持取消收藏）。

## 5. AI 协作记录

- **AI 工具**：opencode（由 Claude 模型驱动的 CLI 交互工具）
- **核心提示词**：用户以分步任务（Task 1 - Task 9）的形式逐条给出需求，明确指定 Store 结构、组件修改内容和证据模板。AI 按照指令读取现有文件、生成代码并写入。
- **AI 生成的内容**：
  - `src/stores/user.ts` 和 `src/stores/favorite.ts` 的完整 Store 定义；
  - `AppHeader.vue` 中用户信息显示区域、Store 导入和样式；
  - `PublishView.vue` 中 Store 导入和 publisher 字段替换（3 处）；
  - `TradeView.vue` 中收藏按钮、Store 导入和样式；
  - `UserCenterView.vue` 和 `MessageView.vue` 的完整模板、脚本和样式。
- **不合理之处**：AI 在 TradeView 的收藏按钮中直接内联了较长的 toggleFavorite 调用对象，可读性一般。后续可以提取为独立方法。

## 6. 人工调整内容

- 确认了 main.ts 中 Pinia 挂载代码已存在，无需额外修改；
- 跳过了 AI 自动生成完整登录系统的部分（Day5 使用模拟用户，不接入真实接口）；
- 确认了四类业务中仅 TradeView 作为示例实现收藏按钮，其他页面可按需扩展；
- 消息中心没有使用 Pinia Store，保持纯展示状态，符合静态页面的定位。

## 7. 测试记录

在本地开发服务器中进行了以下真实测试：

1. 打开二手交易列表页，每条 ItemCard 下方显示"收藏"按钮；
2. 点击"收藏"按钮，按钮文字立即变为"已收藏"，表示该商品已被加入收藏；
3. 再次点击"已收藏"按钮，文字恢复为"收藏"，表示已取消收藏；
4. 进入个人中心页面，在"我的收藏"面板中可以看到刚才收藏的商品卡片，包含标题、描述、分类标签和位置信息；
5. 在个人中心点击"取消收藏"按钮，该商品从收藏列表中移除；
6. 回到二手交易列表页，对应商品的按钮文字恢复为"收藏"；
7. 导航栏右上角正确显示当前用户名"校园用户"。

## 8. 遇到的问题与解决方法

- **问题**：收藏按钮点击后页面无反应。检查后发现 favoriteStore 导入路径错误，写成了 `'./stores/favorite'` 而实际路径应为 `'../stores/favorite'`（TradeView 位于 views 目录下，stores 在 src 根目录）。修正导入路径后功能恢复正常。
- **问题**：刷新页面后收藏数据全部丢失。这是预期行为，因为当前收藏仅保存在前端内存中，未做 localStorage 持久化或后端存储。Day5 阶段可以接受此现象，后续可结合后端接口实现持久化。

## 9. 今日反思

Pinia 状态管理对多页面前端应用的作用十分关键。在没有 Pinia 的情况下，跨组件共享数据只能依赖 prop 逐层传递或 provide/inject，组件一多就难以维护。而通过 Pinia，用户中心和导航栏可以同步读取同一份 userStore，列表页和个人中心可以共享同一份 favoriteStore，修改后的状态在所有引用组件中自动更新，无需手动同步。本次实践中，状态边界的划分也很重要——该共享的数据（用户信息、收藏）放入 Store，局部的数据（表单、校验错误）留在组件内，避免 Store 过度膨胀。用户中心作为多个 Store 的汇聚页面，充分体现了集中式状态管理在信息聚合场景下的优势。
