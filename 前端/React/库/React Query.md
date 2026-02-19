这是一篇为你准备的 **React Query (现更名为 TanStack Query)** 深度学习笔记。它不仅涵盖了基础用法，还整理了答辩中可能用到的核心概念和面试题。

---

# 📝 学习笔记：React Query (TanStack Query)

## 1. 核心定位：它是干什么的？

**一句话定义：** 它是一个用于 React 的**异步状态管理库**（Server State Manager）。

### 💡 核心思想：客户端状态 vs 服务端状态

React Query 的出现是为了解决 Redux 等传统库的痛点，它提倡把状态分为两类：

1. **客户端状态 (Client State):**
* **例子:** 模态框是否打开、侧边栏是否收起、深色模式切换。
* **工具:** `useState`, `useReducer`, Zustand, Redux。
* **特征:** 这种数据**完全由前端控制**，同步更新，即时生效。


2. **服务端状态 (Server State) —— React Query 的地盘:**
* **例子:** 广告列表数据、用户信息、商品详情。
* **特征:** 数据**存放在远程服务器**，前端只是持有一个“快照”。
* **痛点:** 需要处理加载中、报错、缓存过期、数据去重、后台静默更新。



> **比喻：**
> * `useState` 是你口袋里的**记事本**（随手记，自己管）。
> * `React Query` 是图书馆的**图书管理员**（帮你借书、缓存书、定期检查书有没有更新版，过期了自动还书）。
> 
> 

---

## 2. 两大核心 API (CRUD 的基石)

### 🟢 1. `useQuery` —— 用于“读”数据 (GET)

用于获取数据，并自动处理缓存和状态。

```typescript
import { useQuery } from '@tanstack/react-query';
import axios from 'axios';

const { 
  data,       // 后端返回的数据 (成功时有值)
  isLoading,  // 是否正在第一次加载 (无缓存且正在请求)
  isError,    // 是否出错了
  error       // 具体的错误信息
} = useQuery({
  // 🔑 唯一标识符：类似 useEffect 的依赖数组
  // 只要这个数组变了（比如 id 从 1 变 2），React Query 就会自动重新请求
  queryKey: ['ads', id], 
  
  // 🔨 获取数据的函数：必须返回一个 Promise
  queryFn: () => axios.get(`/ads/${id}`).then(res => res.data),
  
  // ⚙️ 配置项 (可选)
  staleTime: 1000 * 60, // 数据 1 分钟内认为是“新鲜”的，不重新发请求
});

```

### 🔵 2. `useMutation` —— 用于“写”数据 (POST/PUT/DELETE)

用于修改服务器数据，比如创建、更新、删除。

```typescript
import { useQueryClient, useMutation } from '@tanstack/react-query';

// 1. 获取管家实例
const queryClient = useQueryClient();

const mutation = useMutation({
  // 🔨 修改数据的动作
  mutationFn: (newAd) => axios.post('/ads', newAd),

  // ✅ 成功后的回调 (非常重要)
  onSuccess: () => {
    // ♻️ 使得缓存失效：告诉管家 'ads' 列表脏了，赶紧去后台重新拉取最新的！
    queryClient.invalidateQueries({ queryKey: ['ads'] });
  },
});

// 使用：
// <button onClick={() => mutation.mutate({ title: '新广告' })}>提交</button>

```

---

## 3. 核心机制原理 (答辩加分项)

### A. 缓存与 Stale-While-Revalidate (SWR) 策略

这是 React Query 的灵魂。

* **Stale (陈旧的):** 数据在 React Query 眼里是有“保质期”的。
* **While-Revalidate (趁机重新验证):**
当你访问一个页面时，如果缓存里有旧数据，React Query 会**先立马显示旧数据**（让用户觉得很快），然后在后台**悄悄发起新请求**。
* 如果你没变：界面保持不变。
* 如果你变了：界面自动更新为新数据。



### B. Query Key (查询键)

`['ads', 'electronics', 1]`
这是一个数组。React Query 依靠这个 Key 来区分不同的数据。这就像 Redis 的 Key 或数据库的主键。

* **层级结构:** 一般遵循 `['资源名', '筛选条件', 'ID']` 的规范。

### C. 两个重要的时间配置

* **`staleTime` (保质期):** 数据多久变“馊”？
* 默认 `0` (认为数据立刻就馊了，每次都会重新请求)。
* 设为 `5000`：5秒内再次切换回来，直接用缓存，**不发请求**。


* **`gcTime` (垃圾回收时间, 旧版叫 cacheTime):** 数据多久被删？
* 默认 `5分钟`。如果你离开页面，数据会在内存里保留5分钟，以防你马上又回来。



---

## 4. 为什么不直接用 `useEffect` + `useState`?

这是面试/答辩中最常见的问题。

| 功能 | 手写 useEffect | React Query |
| --- | --- | --- |
| **代码量** | 繁琐 (需手动维护 loading, error 状态) | 极简 (一行代码) |
| **请求去重** | 难 (组件多次渲染会发多次请求) | ✅ 自动去重 (短时间内只发一次) |
| **缓存管理** | 无 (离开页面数据就丢了) | ✅ 全局缓存 |
| **后台更新** | 无 (切回来页面不会自动刷新) | ✅ 窗口聚焦时自动刷新 (Window Focus Refetching) |
| **竞态问题** | 需手动处理 (快速点击分类) | ✅ 自动处理 (只保留最后一次请求结果) |

---

## 5. 项目实战总结 (Mini Ad Wall)

在你的广告墙项目中，React Query 的数据流如下：

1. **挂载:** `AdList` 组件加载，`useQuery(['ads'])` 触发，状态为 `isLoading`。
2. **显示:** 从后端拉取到 JSON，状态变为 `success`，页面渲染列表。
3. **用户操作:** 用户点击“创建广告”，调用 `useMutation`。
4. **提交:** `mutation.mutate()` 向后端发 POST 请求。
5. **同步:** 请求成功 (`onSuccess`)，执行 `queryClient.invalidateQueries(['ads'])`。
6. **自动刷新:** `useQuery` 监听到 `['ads']` 被标记为陈旧 (Stale)，立刻在后台自动重发 GET 请求。
7. **更新:** 列表自动更新为包含新广告的最新状态。

---

## 6. 答辩话术小抄

> **Q: 你为什么在项目中引入 React Query？**
> **A:** "在项目中，为了更好的分离**UI状态**和**服务端数据状态**，我引入了 React Query。
> 1. 它极大地**简化了代码**，我不需手动编写大量的 `isLoading` 和 `useEffect` 逻辑。
> 2. 它提供了开箱即用的**缓存策略**和**自动重试**机制，提升了用户体验。
> 3. 特别是它的 **Invalidation（缓存失效）** 机制，让我在提交表单后能自动刷新列表，保证了前后端数据的一致性。"

