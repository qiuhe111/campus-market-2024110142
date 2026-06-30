# Day4 Evidence - 发布表单与数据新增

## 1. 今日完成内容

今天完成了四种业务类型的发布表单页面（PublishView.vue），实现了：

- 创建了 `FormField.vue` 基础表单项组件，统一管理标签、必填标识和错误提示
- 设计了包含通用字段（标题、地点、描述）和四种业务类型专属字段的发布表单
- 实现了完整的表单校验逻辑（`clearErrors` + `validateForm`）
- 实现了四种业务的数据提交逻辑（`handleSubmit`），调用对应的 `createXxx` API 将数据写入 JSON Server
- 实现了表单重置功能（`resetForm` + 清除错误提示）
- 添加了页面样式，包含卡片式布局和按钮交互状态

## 2. 发布类型与字段设计

| 发布类型 | 对应数据集合 | 关键字段 | 设计理由 |
|---|---|---|---|
| 二手交易 | trades | title、category、price、condition、location、description | 需要展示商品基本信息和交易条件，分类方便筛选，成色影响买家决策 |
| 失物招领 | lostFounds | title、type、itemName、location、eventTime、description | 需要区分寻物和招领，物品名称是匹配关键，发生时间帮助判断时效 |
| 拼单搭子 | groupBuys | title、type、targetCount、deadline、location、description | 需要说明拼单类型和目标人数，截止时间控制拼单有效期，currentCount 初始化为 1 |
| 跑腿委托 | errands | title、taskType、reward、from、to、deadline、description | 需要描述任务类型和酬劳，取件地点和送达地点是执行关键，截止时间设定时效 |

四种业务共享的通用字段为 **标题、地点、描述**，这些是所有校园信息发布场景必不可少的核心信息。业务专属字段则根据 `db.json` 中已有数据结构的字段设计，确保提交时与 mock 数据格式完全对齐。

## 3. 表单校验规则

- **通用字段**：标题、地点、描述均不能为空，为空时分别提示"请输入标题""请输入地点""请输入描述"
- **二手交易**：商品分类不能为空；价格必须大于 0；成色不能为空（需从下拉列表选择）
- **失物招领**：物品名称不能为空；发生时间不能为空（需通过 datetime-local 选择器选择）
- **拼单搭子**：拼单类型不能为空；目标人数不能少于 2 人；截止时间不能为空
- **跑腿委托**：任务类型不能为空；酬劳不能为负数（可为 0，表示免费帮忙）；取件地点和送达地点均不能为空；截止时间不能为空

校验在表单提交时统一执行，使用 `clearErrors` 先清空所有错误信息，再逐字段检查。校验不通过时，对应 `FormField` 组件会显示红色错误提示文本，引导用户补充填写。

## 4. AI 协作记录

- **使用的 AI 工具**：opencode（基于 Claude 模型）
- **核心提示词**：
  1. "在已有 API 文件基础上，为每类业务补充新增方法"——生成了四个 API 文件的 `createXxx` 方法
  2. "创建基础表单项组件"——生成了 `FormField.vue` 组件
  3. 提供了 `PublishView.vue` 的 template 片段，AI 补全了完整的 `<script setup>` 和 `<style>`，包括响应式数据、校验函数、提交函数
  4. "根据发布类型显示不同字段"——AI 在表单中添加了四种业务类型的条件模板
  5. "实现基础表单校验""实现数据提交逻辑""实现表单重置"——AI 逐步完善了校验、提交和重置逻辑
  6. "补充发布页面样式"——AI 替换了页面样式为卡片式布局
- **AI 生成的代码**：`FormField.vue`、`PublishView.vue`（完整表单页面，含 template/script/style）、各 API 文件的 `createXxx` 方法
- **AI 生成内容中的不合理之处**：最初生成的 `reward` 校验使用了 `<= 0`（酬劳必须大于 0），但实际校园场景中可能存在免费帮忙（酬劳为 0）的情况，后来调整为 `< 0`；初始样式中的主按钮颜色为 `#6366f1`（靛蓝），与项目整体蓝色调不太协调，后改为 `#2563eb`

## 5. 人工调整内容

- **硬编码占位值处理**：将 `publisher` 固定为"当前用户"（后续接入登录系统后替换为真实用户信息）；`image` 设为空字符串（暂不实现图片上传）
- **校验规则调整**：`reward` 的校验从 `<= 0` 改为 `< 0`，允许酬劳为 0 表示免费帮助
- **样式颜色调整**：主按钮颜色从 `#6366f1` 改为 `#2563eb`，与项目整体蓝色系保持一致
- **表单数据字段对齐**：确保 `form.groupType` 提交时映射为 `type`，`form.lostFoundType` 提交时映射为 `type`，与 `db.json` 中的字段名一致
- **代码结构优化**：将清空错误信息提取为独立的 `clearErrors()` 函数，避免在每个校验分支中手动清空对应字段

## 6. 测试记录

使用 curl 模拟四种业务的表单提交，对 JSON Server 进行了完整发布流程测试：

**测试一：二手交易发布**
- 请求：`POST /trades`，数据包含 title="测试键盘"、category="数码配件"、price=50、condition="八成新" 等
- 结果：返回 201 Created，自动生成 id="v1lgGKgaKrE"
- 验证：`db.json` 中 trades 数组末尾出现新记录，字段完整

**测试二：失物招领发布**
- 请求：`POST /lostFounds`，数据包含 type="lost"、itemName="水杯" 等
- 结果：返回 201 Created，自动生成 id="WLj63PH4J3s"
- 验证：`db.json` 中 lostFounds 数组新增记录正常

**测试三：拼单搭子发布**
- 请求：`POST /groupBuys`，数据包含 targetCount=4、currentCount=1 等
- 结果：返回 201 Created，自动生成 id="4DgEqMzrn0U"
- 验证：`db.json` 中 groupBuys 数组新增记录正常

**测试四：跑腿委托发布**
- 请求：`POST /errands`，数据包含 reward=5、from="菜鸟驿站"、to="西区5栋" 等
- 结果：返回 201 Created，自动生成 id="zQnQDkpxG1k"
- 验证：`db.json` 中 errands 数组末尾新增记录正常

## 7. 遇到的问题与解决方法

**问题：PowerShell 中 JSON 字符串传参异常**

在使用 `curl.exe` 通过 PowerShell 发送 POST 请求时，JSON 数据中的中文字符在命令行中解析失败，提示 "Expected property name or '}' in JSON at position 1"。这是因为 PowerShell 的双引号嵌套和编码处理与 cmd 不同。

**解决方法**：改用 PowerShell 原生方式构造 JSON——先将数据定义为 `Hashtable`，通过 `ConvertTo-Json` 转换为 JSON 字符串，再通过 `Invoke-WebRequest -UseBasicParsing` 发送请求，避免了手动拼接 JSON 字符串的编码问题。例如：

```powershell
$body = @{title="测试键盘"; category="数码配件"} | ConvertTo-Json
Invoke-WebRequest -Uri "http://localhost:3001/trades" -Method Post -ContentType "application/json" -Body $body -UseBasicParsing
```

## 8. 今日反思

**发布表单**是用户与系统交互的主要入口，它将用户意图转化为结构化数据。一个好的发布表单应该：字段设计贴合业务场景、操作路径清晰、反馈及时。**表单校验**是数据质量的第一道防线，在前端拦截常见的空值、格式错误和业务逻辑违规（如价格必须为正数、目标人数不能少于 2 人），避免无效数据进入后端。**数据新增**则是 CRUD 中 Create 操作的具体实现，它将校验通过的表单数据通过 API 写入数据源（这里为 JSON Server 的 db.json），完成从用户输入到数据持久化的完整闭环。

三者共同构成了 Web 应用数据生命周期的基础环节：用户通过发布表单输入 → 表单校验确保数据合规 → 数据新增将有效数据写入存储。在此基础上，后续的列表展示、详情查看、编辑更新等功能才能正常运行。因此，发布表单的设计质量直接影响用户体验，表单校验的严谨程度决定数据质量，而数据新增的稳定性则关系整个应用的数据完整性。
