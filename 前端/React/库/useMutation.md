`useMutation` 是前端状态管理库 **React Query (TanStack Query)** 中的一个核心 Hook。

如果说 `useQuery` 是用来 **“读 (Read)”** 数据的（比如获取广告列表），那么 `useMutation` 就是专门用来 **“写 (Write)”** 数据的（比如**创建**广告、**修改**广告、**删除**广告）。

你可以把它理解为**“数据修改器”**。

在你的 **Mini Ad Wall** 项目中，当你点击“提交”按钮创建新广告时，背后在工作的往往就是 `useMutation`。

---

### 1. 为什么需要它？(痛点分析)

如果不使用 `useMutation`，你在 React 里写一个“提交表单”的功能，通常要写这么多重复的样板代码：

```javascript
// ❌ 传统的笨办法
const [isLoading, setIsLoading] = useState(false);
const [error, setError] = useState(null);

const handleSubmit = async (data) => {
  setIsLoading(true); // 1. 手动开启 loading
  setError(null);     // 2. 清空旧错误
  try {
    await axios.post('/ads', data);
    alert('成功！'); // 3. 处理成功
    // 4. 手动去刷新列表...
  } catch (err) {
    setError(err);   // 5. 捕获错误
  } finally {
    setIsLoading(false); // 6. 手动关闭 loading
  }
};

```

**`useMutation` 的作用就是把这 6 步全部封装起来，让你一行代码搞定状态管理。**

---

### 2. 核心语法结构

`useMutation` 接收一个配置对象，返回一个包含状态和触发器的对象。

```typescript
// ✅ React Query 的写法
const mutation = useMutation({
  // A. 核心动作：你要干什么？(必须返回一个 Promise)
  mutationFn: (newAdData) => {
    return axios.post('/ads', newAdData);
  },

  // B. 生命周期：成功了怎么办？
  onSuccess: () => {
    console.log('创建成功！');
    // 神奇之处：通知 queryClient 刷新广告列表
    queryClient.invalidateQueries({ queryKey: ['ads'] });
  },

  // C. 生命周期：失败了怎么办？
  onError: (error) => {
    console.error('出错了：', error);
  }
});

```

---

### 3. 解剖返回值：它给了你什么？

当你调用 `const mutation = useMutation(...)` 后，`mutation` 对象里有几个极其重要的属性供你在 UI 中使用：

* **`mutation.mutate(data)`**: **这是那个开关！** 调用这个函数，请求才会真正发出去。你把它绑定在按钮的 `onClick` 上。
* **`mutation.isPending`** (旧版叫 `isLoading`): 一个布尔值。请求正在发吗？如果是 `true`，你就让按钮转圈圈，并禁用按钮防止重复点击。
* **`mutation.isError`**: 只要请求失败，它就是 `true`。
* **`mutation.error`**: 具体的错误信息对象。
* **`mutation.data`**: 如果请求成功，后端返回的数据（比如 `{ id: 101, status: 'ok' }`）就在这里。

---

### 4. 结合你的项目实战

让我们看看在你的 **Mini Ad Wall** 的 `AdModal.tsx`（创建广告弹窗）中，`useMutation` 是如何工作的：

```tsx
// 1. 定义 Mutation
const createAdMutation = useMutation({
  mutationFn: (values) => axios.post('/api/ads', values),
  onSuccess: () => {
    message.success('广告发布成功！');
    // 关键：告诉 React Query "ads" 这个列表脏了，赶紧去后台重新拉取最新的！
    queryClient.invalidateQueries({ queryKey: ['ads'] });
    closeModal(); // 关闭弹窗
  }
});

// 2. 在 UI 中使用
return (
  <Modal 
    title="发布广告"
    confirmLoading={createAdMutation.isPending} // ✨ 自动关联 loading 状态
    onOk={() => {
      // 提交表单时，调用 mutate 触发请求
      form.validateFields().then(values => {
        createAdMutation.mutate(values); 
      });
    }}
  >
    {/* ...表单内容... */}
    {createAdMutation.isError && (
       <div style={{color: 'red'}}>发布失败: {createAdMutation.error.message}</div>
    )}
  </Modal>
);

```

### 5. 高级特性：Mutate vs MutateAsync

在答辩或面试中，可能会问到 `mutation.mutate` 和 `mutation.mutateAsync` 的区别：

* **`mutate(variables)`**:
* 它是**“射后不理” (Fire and Forget)**。
* 你调用它之后，它在后台跑。成功或失败的逻辑，你主要写在 `onSuccess` / `onError` 回调里。
* 它没有返回值（或者说你拿不到 Promise 结果）。


* **`mutateAsync(variables)`**:
* 它返回一个 **Promise**。
* 这就允许你在 UI 组件里写 `await`。
* **场景：** 比如你需要等待创建成功后，拿到新广告的 ID，立马跳转到详情页 `/ads/:id`。


```javascript
// 使用 mutateAsync 的场景
const result = await createAdMutation.mutateAsync(data);
navigate(`/ads/${result.data.id}`); // 跳转

```



### 6. 答辩核心话术

如果老师问：“你在项目中是怎么处理数据提交的？”

你可以回答：

> “我使用了 React Query 的 **`useMutation`** Hook 来管理所有的数据修改操作（增删改）。
> 相比于手动维护 loading 和 error 状态，`useMutation` 提供了标准化的生命周期管理。
> 尤其是它的 **`onSuccess` 回调** 配合 **`queryClient.invalidateQueries`**，实现了一个非常强大的功能：**当我在前台添加一个广告成功后，后台会自动触发列表的重新拉取**。这保证了用户看到的列表永远是最新的，不需要手动刷新页面。”

### 总结

**`useMutation` = 异步动作触发器 + 自动状态机 + 结果回调系统。**
它是 React Query 中处理**副作用 (Side Effects)** 的核心工具。