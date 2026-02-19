如果把 `fetch` 比作一把**瑞士军刀**（原生、够用、但需要自己动手），那么 Axios 就是一套**全自动化的现代厨房设备**（功能强大、省心、开箱即用）。

它可以同时在 **浏览器端** 和 **Node.js 服务端** 运行。在前端开发中，它是目前最流行的请求库。

以下是 Axios 能做的核心事情，以及它比 `fetch` 强在哪里的具体分析：

---

### 1. 核心功能（它能做什么？）

#### ✅ 1. 自动转换 JSON 数据 (最省心)

- **Fetch:** 你必须手动写 `JSON.stringify(data)` 发送数据，收到后还要写 `await res.json()` 解析数据。
    
- **Axios:** 自动帮你做。你传对象，它发 JSON；后端回 JSON，它自动转成对象。
    

#### ✅ 2. 请求与响应拦截器 (Interceptors) —— 🔥 杀手级功能

这是企业级项目必选 Axios 的最大理由。它允许你在请求**发出前**和响应**收到后**，统一做一些事情。

- **请求拦截:** 自动给所有请求头加上 `Authorization: Bearer token`。
    
- **响应拦截:** 统一处理错误（比如遇到 401 token 过期，自动跳转登录页；遇到 500，统一弹窗提示“服务器出错”）。
    

#### ✅ 3. 更合理的错误处理

- **Fetch:** 只要网络通了，哪怕是 404 或 500，它都认为是“成功”（resolve），你需要手动判断 `if (!res.ok)`。
    
- **Axios:** 只要状态码不在 2xx 范围内（比如 404, 500），它会直接抛出错误（reject），你可以直接在 `catch` 里捕获。这更符合直觉。
    

#### ✅ 4. 实例与配置 (Instances)

你可以创建一个 `apiClient` 实例，配置好 `baseURL`（比如 `https://api.myapp.com`）和超时时间。以后发请求只需要写相对路径 `/users`，不用每次都写长长的 URL。

#### ✅ 5. 上传/下载进度监控 (Progress)

Axios 提供了 `onUploadProgress` 和 `onDownloadProgress` 回调，做**文件上传进度条**非常简单。这是 `fetch` 很难做到的。

#### ✅ 6. 取消请求 (Cancellation)

虽然 `fetch` 也有 `AbortController`，但 Axios 将其封装得稍微好用一点（早期用 CancelToken，现在也支持 AbortController）。

---

### 2. 代码对比：Axios vs Fetch

#### 场景一：发送 POST 请求

**使用 Fetch (比较繁琐):**

```js
const createUser = async (userData) => {
  try {
    const response = await fetch('https://api.example.com/users', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json', // 必须手动设置
      },
      body: JSON.stringify(userData), // 必须手动转字符串
    });

    if (!response.ok) { // 必须手动检查状态码
      throw new Error('HTTP error ' + response.status);
    }

    const data = await response.json(); // 必须手动解析
    return data;
  } catch (error) {
    console.error(error);
  }
};
```

**使用 Axios (简洁):**

```js
import axios from 'axios';

const createUser = async (userData) => {
  try {
    // 自动 stringify，自动设置 Content-Type，自动 parse 响应
    const response = await axios.post('https://api.example.com/users', userData);
    return response.data; // 数据直接在 data 属性里
  } catch (error) {
    // 404/500 会直接跳到这里
    console.error(error);
  }
};
```

---

#### 场景二：企业级封装 (拦截器演示)

这是 Axios 最强大的地方，通常我们会封装一个 `request.js` 文件：

```js
// src/utils/request.js
import axios from 'axios';

// 1. 创建实例
const service = axios.create({
  baseURL: 'https://api.example.com', // 基础路径
  timeout: 5000, // 超时时间 5秒
});

// 2. 请求拦截器 (Request Interceptor)
service.interceptors.request.use(
  (config) => {
    // 在发送请求之前做些什么
    const token = localStorage.getItem('token');
    if (token) {
      // 自动给所有请求加上 Token
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  (error) => Promise.reject(error)
);

// 3. 响应拦截器 (Response Interceptor)
service.interceptors.response.use(
  (response) => {
    // 2xx 范围内的状态码都会触发该函数
    return response.data; // 直接返回数据部分，过滤掉 header 等信息
  },
  (error) => {
    // 超出 2xx 范围的状态码都会触发该函数
    if (error.response) {
      switch (error.response.status) {
        case 401:
          // Token 过期，跳转登录页
          window.location.href = '/login';
          break;
        case 500:
          alert('服务器崩了，请稍后再试');
          break;
        default:
          console.error('发生错误');
      }
    }
    return Promise.reject(error);
  }
);

export default service;
```

---

### 3. 总结：你应该用哪个？

|**特性**|**Fetch**|**Axios**|
|---|---|---|
|**原生支持**|✅ (浏览器/Node18+ 内置)|❌ (需要 `npm install`)|
|**体积**|0KB|约 10KB (Gzipped)|
|**语法**|稍微繁琐，偏底层|简洁，偏应用层|
|**错误处理**|仅网络中断才报错|HTTP 错误状态码也报错|
|**拦截器**|无 (需手动封装)|**强大且易用**|
|**上传进度**|❌ (不支持)|✅ (支持)|

**建议：**

- **个人小 Demo / 极简项目**：用 **Fetch**。原生、无依赖，够用了。
    
- **中大型企业项目 / 团队协作**：**必选 Axios**。拦截器、统一错误处理、请求封装能帮你省下大量的重复代码，维护起来也更方便。

目前我理解只需要掌握怎么写HTTP请求方法，会看状态码就足够完成增删查改业务了，训练营有更高的要求吗