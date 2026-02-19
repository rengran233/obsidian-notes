在你的 **Mini Ad Wall** 项目中，**路由 (Route)** 和 **API** 是前后端交互的“交通枢纽”和“通用语言”。在答辩中，这两个概念通常是结合在一起讲的。

为了让你在面试官面前表现得专业，我结合你的代码 `adRoutes.ts` 和 `ads.ts` 来具体解释它们的作用。

---

### 1. 路由 (Route): “交通指挥官”

**路由的作用是：把用户请求的 URL 地址，精确地“指派”给对应的处理函数（Controller）。**

在你的项目中，后端服务器就像一栋大楼，路由就是大楼门口的**前台指引牌**。

#### **代码实证**

请看你的 `server/src/routes/adRoutes.ts` 文件：
```TypeScript
// 1. 定义前缀：只要是 /ads 开头的请求，都来找我
const router = new Router({ prefix: '/ads' });

// 2. 指路逻辑 (路由映射)
// 如果有人通过 GET 方法访问根目录 (即 GET /ads) -> 找 AdController.getAds 去处理
router.get('/', AdController.getAds);           

// 如果有人通过 POST 方法访问 /upload -> 找 upload.single 和 AdController.uploadFile
router.post('/upload', upload.single('file'), AdController.uploadFile);

// 如果有人通过 POST 方法访问 /:id/click -> 找 AdController.clickAd
router.post('/:id/click', AdController.clickAd); 
```

#### **路由的核心职责：**

1. **分发请求 (Dispatch):** 区分用户的意图。同样是访问 `/ads`，如果是 `GET` 就是“我要看广告”，如果是 `POST` 就是“我要发广告”。路由通过 **HTTP Method (GET/POST/PUT/DELETE)** 来区分。
2. **解析参数 (Parsing):** 在 `/:id/click` 中，路由负责识别出 `:id` 是一个动态参数（比如 `uuid-123`），并把它传给 Controller。

---

### 2. API (Application Programming Interface): “服务窗口”

**API 的作用是：定义前后端如何对话的“契约”或“规则”。**

如果说后端是“厨房”，前端是“顾客”，那 API 就是**菜单**和服务员。它规定了前端必须怎么点菜（传什么参数），以及后端会端出来什么菜（返回什么数据格式）。

#### **代码实证**

你的项目中采用了 **RESTful API** 风格。
- **前端定义 (发起方):** 在 `client/src/api/ads.ts` 中。
```TypeScript
// 前端严格遵守 API 契约
clickAd: async (id: string) => {
  // 我知道后端规定了 POST /ads/:id/click 这个窗口
  const res = await apiClient.post<ApiResponse<Ad>>(`/ads/${id}/click`);
  return res.data.data;
},
```
- **后端响应 (接收方):** 在 `server/src/controllers/adController.ts` 中。
```TypeScript
// 后端履行 API 契约
async clickAd(ctx: Context) {
  // ...处理逻辑...
  // 按照约定返回 JSON 格式：code, data, message
  ctx.body = { code: 0, data: updatedAd, message: 'Click count incremented' };
},
```

#### **API 的核心职责：**

1. **数据交换:** 它是数据流通的管道。前端把表单数据通过 API 传给后端，后端把数据库里的广告列表通过 API 传回给前端。
2. **解耦 (Decoupling):** 前端不需要知道后端是用 Node.js 写的还是 Java 写的，也不需要知道数据库是 SQLite 还是 MySQL。前端只需要知道 **API 地址** 和 **JSON 格式**，就能工作。

---

### 3. 它们是如何配合工作的？（答辩流程图）

在答辩时，如果面试官问“用户点击一个广告后发生了什么？”，你可以用这个流程串联路由和 API：
1. **用户动作:** 用户在浏览器点击了广告卡片。
2. **前端 API 调用:** 前端代码执行 `adApi.clickAd(id)`，通过 **Axios** 发起一个 HTTP 请求。
    - _请求目标:_ `POST http://localhost:3000/ads/123/click`
3. **网络传输:** 请求穿过互联网（或本地网络）到达后端服务器。
4. **路由匹配 (Router):**
    - Koa 的路由层拦截到请求。
    - 它发现 URL 匹配 `POST /ads/:id/click`。
    - 于是它大喊：“这是点击操作！`AdController.clickAd`，该你上场了！”
5. **业务处理:** Controller 调用 Service 修改数据库。
6. **API 响应:** 后端把最新的点击数封装成 JSON，原路返回给前端。

---

### 💡 餐厅比喻 (秒懂版)

为了让解释更生动，你可以使用这个比喻：
- **API (接口):** 是餐厅的**菜单**。它告诉顾客（前端）我们提供什么服务（获取列表、上传视频），以及你需要付多少钱（传什么参数）。
- **Router (路由):** 是餐厅的**迎宾员**或**调度系统**。
    - 当服务员拿着订单（请求）过来，迎宾员看一眼单子：“哦，这是要点菜（POST），去厨房找大厨（Controller）”；
    - 或者“这是要结账（GET），去柜台找收银员”。
    - 路由确保请求不会走错房间。

### 总结
- **路由**是后端的**地图**，负责把请求**导向**正确的代码函数。
- **API** 是前后端的**协议**，负责规定数据**怎么传、传什么**。