`ctx` 是 **Context（上下文）** 的缩写，它是 Koa 框架中最核心、最独特的一个概念。

你可以把 `ctx` 想象成一个 **“智能公文包”** 或服务员手中的 **“托盘”**。

当一个 HTTP 请求打到你的服务器时，Koa 会立刻创建一个新的 `ctx` 对象。这个对象会**伴随这个请求的一生**（从进入服务器，穿过所有的中间件，直到离开服务器返回给用户），里面装了这次请求所有的信息和工具。

为了让你彻底搞懂，我将其拆解为 **4 个核心维度**：

---

### 1. 它的核心身份：封装者 (The Wrapper)

在原生的 Node.js (`http` 模块) 中，处理请求需要两个参数：`req` (请求) 和 `res` (响应)。

```javascript
// 原生 Node.js (很麻烦)
server.on('request', (req, res) => { ... });

```

Koa 觉得这样太麻烦，于是创造了 `ctx`，把这两个东西合二为一：
**`ctx` = `Request` (入) + `Response` (出) + `Tools` (工具)**

* **入 (Input):** 用户发来了什么？（URL、参数、Header、Body）
* **出 (Output):** 你要回给用户什么？（JSON、HTML、状态码）
* **私货 (State):** 中间件之间想悄悄传递什么数据？

---

### 2. 它的内部结构：三个重要属性

在你的代码中，打印 `console.log(ctx)`，你会发现它主要由这三部分组成：

#### A. `ctx.request` (Koa 请求对象)

这是 Koa 封装过的请求对象，比原生的 `req` 更好用。

* **`ctx.request.url`**: 请求地址 (如 `/ads`)。
* **`ctx.request.method`**: 请求方法 (如 `GET`, `POST`)。
* **`ctx.request.body`**: **重点！** 如果你用了 `koa-bodyparser`，前端发的 JSON 数据就在这里。
* **`ctx.request.header`**: 请求头信息。

#### B. `ctx.response` (Koa 响应对象)

这是 Koa 封装过的响应对象，用来控制你给浏览器回什么。

* **`ctx.response.status`**: 状态码 (默认 404，赋值后变 200)。
* **`ctx.response.body`**: **重点！** 你把数据赋值给它，Koa 就会自动发给浏览器。

#### C. `ctx.state` (共享空间)

这是一个空的“口袋”，专门用来**在中间件之间传递数据**。

* **场景：** 比如第一个中间件是 `Auth`（鉴权），它解密了 token 里的用户信息 `user_id: 1`。它不能直接传参给下一个函数，怎么办？
* **做法：** 把它塞进口袋 `ctx.state.user = { id: 1 }`。
* **结果：** 后面的 `AdController` 就可以直接从 `ctx.state.user` 里拿数据了。

---

### 3. 它的“魔法”：委托 (Delegation / Aliases)

Koa 为了让你写代码少打几个字，做了一件很贴心的事：**委托**。
它把 `ctx.request` 和 `ctx.response` 上常用的属性，直接挂到了 `ctx` 身上。

| 你写的代码 (简写) | Koa 实际执行的代码 (全称) | 作用 |
| --- | --- | --- |
| `ctx.body = 'hello'` | `ctx.response.body = 'hello'` | **设置**返回内容 |
| `ctx.status = 200` | `ctx.response.status = 200` | **设置**状态码 |
| `ctx.url` | `ctx.request.url` | **读取**请求路径 |
| `ctx.method` | `ctx.request.method` | **读取**请求方法 |
| `ctx.query` | `ctx.request.query` | **读取** URL 参数 (`?id=1`) |

**注意：** 并不是所有属性都有简写。比如 `ctx.request.body` 就通常没有简写成 `ctx.body`（因为 `ctx.body` 已经被占用来做响应了），所以**获取 POST 数据必须写全 `ctx.request.body**`。

---

### 4. 实战：在你的 Ad Wall 项目中怎么用？

让我们回到你的 `AdController.ts`，看看 `ctx` 是怎么大显身手的：

```typescript
// 场景：创建一个新广告
static async createAd(ctx: Context) {
    // 1. 【入】获取请求体 (必须用 request.body)
    // 前端发来的 JSON: { title: "买可乐", content: "..." }
    const { title, content } = ctx.request.body; 

    // 2. 【入】获取路由参数 (这是 @koa/router 挂载的特殊属性)
    // 假设路由是 /ads/:id，前端请求 /ads/99
    // const id = ctx.params.id; 

    // ... 执行 Prisma 数据库操作 ...

    // 3. 【出】设置响应状态码 (使用简写)
    ctx.status = 201; // Created

    // 4. 【出】返回数据给前端 (使用简写)
    // Koa 会自动把这个对象转成 JSON 字符串发给浏览器
    ctx.body = {
        success: true,
        message: '广告创建成功',
        data: { id: 101, title }
    };
}

```

### 5. 易混淆点：`ctx.req` vs `ctx.request`

在 `ctx` 里，你会同时看到 `req` 和 `request`，这很容易让人晕。

* **`ctx.req` (Node 原生对象):** 这是 Node.js 最底层的 HTTP 请求对象。它是只读流，很难用，基本是给库作者用的。**尽量别碰它。**
* **`ctx.request` (Koa 对象):** 这是 Koa 帮你加工过的、好用的对象。**请只用这个。**

### 🎓 答辩总结

如果老师问：“为什么要用 `ctx` 而不是直接用 Node 的 `req` 和 `res`？”

你可以回答：

> “`ctx` 是 Koa 对 HTTP 请求上下文的高度封装。
> 1. **整合性：** 它将请求 (`request`) 和响应 (`response`) 整合在一个对象中，方便在中间件链路中传递。
> 2. **易用性：** 它提供了大量便捷的**别名 (Aliases)**，比如 `ctx.body` 和 `ctx.query`，简化了 API 的调用。
> 3. **状态共享：** 它提供了 `ctx.state` 命名空间，是中间件之间安全共享数据（如用户信息）的标准方式。”