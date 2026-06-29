# Day3 Evidence - Mock 数据建模与列表渲染

## 1. 今日完成内容

今天完成了校园轻集市项目的 Mock 数据层搭建和列表渲染工作。首先设计了 db.json 中四类业务的数据结构，包含二手交易（trades）、失物招领（lostFounds）、拼单搭子（groupBuys）和跑腿委托（errands），每类业务各提供 5 条贴近校园生活的 Mock 数据。然后使用 JSON Server 作为 Mock API 启动服务，并在项目中封装了 Axios 基础请求实例，按业务模块拆分了独立的 API 请求文件。最后在四个对应的 Vue 页面中完成了数据请求和列表渲染，并添加了空状态组件和通用卡片组件。

## 2. Mock 数据结构说明

| 数据集合 | 对应业务 | 主要字段 | 页面用途 |
|---|---|---|---|
| trades | 二手交易 | title、price、category、location、status | 展示二手商品列表 |
| lostFounds | 失物招领 | title、type、itemName、location、eventTime、status | 展示失物和招领信息 |
| groupBuys | 拼单搭子 | title、type、targetCount、currentCount、deadline | 展示拼单和搭子信息 |
| errands | 跑腿委托 | title、taskType、reward、from、to、deadline | 展示跑腿任务列表 |

## 3. 我的设计

在设计数据结构时，我围绕四个核心问题来思考每个字段的必要性。

对于二手交易，我设计了 price 和 condition 两个关键字段。price 是交易的核心要素，买家需要直观看到价格来快速筛选；condition（成色）则是二手交易特有的属性，全新、九成新、七成新直接决定了商品的估值，用户必须知道成色才能决定是否进一步查看详情。

对于失物招领，我特意设计了 type 字段并用 'lost' 和 'found' 两个值来区分"寻物"和"拾物"两种场景。这是失物招领业务最核心的区分维度——用户要么丢了东西想发布寻找，要么捡到东西想发布招领。如果没有 type 字段，两种信息混在一起会让页面难以使用。同时我保留了 eventTime（事发时间）而非用 publishTime，因为匹配失物的关键是"什么时候丢的/捡的"而不是"什么时候发布的"。

对于拼单搭子，我设计了 targetCount 和 currentCount 两个字段来记录人数进度。拼单的本质是"凑够人数"，用户需要知道还差几个人才能成团，这两个数字的对比可以直观展示拼单的热度和紧迫感。deadline 字段也很关键，因为拼单通常有截止时间限制。

对于跑腿委托，我设计了 from、to 和 reward 三个字段。跑腿业务的核心是"从哪到哪"的路线信息和"报酬"——用户决定是否接单时最关注的就是距离远近和报酬多少。taskType 字段则用来区分取快递、代买、占座等不同任务类型。

所有数据集合都保留了 status 字段，值为 "open"，为后续开发"已关闭/已完成"等状态管理预留扩展点。

## 4. AI 设计

AI 工具帮我生成了以下内容：

第一，根据 db.json 的字段定义，AI 生成了每个业务模块的 TypeScript 接口。例如 TradeItem 接口完整映射了 title、price、category、condition、location、publishTime、status、description 等字段，类型定义准确，可以直接在组件中使用。

第二，AI 生成了按业务拆分的 API 请求模块（src/api/trade.ts、lostFound.ts、groupBuy.ts、errand.ts），每个模块都导出了对应的 getXxx() 请求函数，代码简洁，没有多余的封装。

第三，AI 生成了页面组件中的列表渲染代码，在 TradeView.vue 等四个页面中使用了 onMounted 生命周期请求数据，并用 v-for 循环渲染 ItemCard 组件。

AI 生成内容的不合理之处：一是四个 API 模块目前只生成了 GET 请求方法，缺少后续需要的 POST/PUT/DELETE 方法；二是 http.ts 中的 baseURL 直接写死了 localhost:3001，没有使用环境变量；三是页面组件中没有处理 loading 状态和错误捕获，如果 JSON Server 没有启动，页面不会有任何反馈。

## 5. 最终调整

我在 AI 生成的基础上做了以下调整：

第一，补充了 EmptyState 空状态组件。AI 生成的页面列表在数据为空时只有一个空的 div.list，没有任何提示。我在四个页面中添加了 EmptyState 组件，当数据数组长度为 0 时显示"暂无xx信息"的友好提示。

第二，调整了 ItemCard 组件的字段映射。AI 最初对所有卡片使用相同的字段映射方式，我发现不同业务需要展示不同信息：TradeView 应该突出价格和成色，GroupBuyView 应该展示拼单进度，ErrandView 应该展示报酬和送达地址。所以我通过 footer slot 为每个页面定制了底部信息展示。

第三，调整了 LostFoundView 的 tag 显示。AI 直接使用了 item.type 作为 tag 值，但 'lost' 和 'found' 对用户不友好，我改为使用三元表达式显示"寻物"和"拾物"。

第四，将证据卡从 docs/Day3-证据卡.md 移动到 docs/evidence/Day3_Evidence.md，与项目规范保持一致。

## 6. 遇到的问题与解决方法

遇到的一个真实问题是 JSON Server 启动后被自动杀掉。我使用 npm run mock 命令启动 JSON Server，但终端工具设置了 10 秒超时，导致 json-server 进程在 10 秒后被强制终止。当我尝试访问 http://localhost:3001/ 时浏览器提示无法连接。

解决方法：使用 Start-Process 在后台启动 json-server，让它独立于终端的超时限制运行。具体是通过 cmd.exe 调用 npm run mock 并将进程放在独立窗口中运行。之后还需要先杀掉旧进程再启动新进程，避免端口占用冲突。

## 7. 今日反思

Mock 数据建模、JSON Server 接口服务和列表渲染是项目开发的基础设施建设。有了规范的 Mock 数据，前端开发可以脱离后端并行推进；有了分层的 API 模块，页面组件不必关心请求细节，代码更加可维护；有了列表渲染的完整链路，后续的功能开发（详情页、发布页、筛选搜索）都可以在此基础上快速扩展。今天搭建的这一层不仅是 Day3 的交付物，更是整个项目后续迭代的基石。
