React的事件系统主要靠合成事件和事件委托运行
### 1. 合成事件 (SyntheticEvent) —— “抹平差异的中间件”

在 React 中，当你写 `onClick={(e) => ...}` 时，回调函数里接收到的这个 `e`，**不是** 浏览器原生的 `Event` 对象，而是一个 React 自己创建的对象，叫 `SyntheticEvent`。

#### 为什么要造个假对象？
*   **跨浏览器兼容 (W3C 标准化)**：
    *   浏览器（尤其是早期的 IE）对事件的实现各不相同。有的叫 `event`，有的叫 `window.event`；阻止冒泡有的用 `stopPropagation()`，有的用 `cancelBubble = true`。
    *   React 帮你处理了所有脏活。`SyntheticEvent` 严格遵循 W3C 标准。你在 React 里写的 `e.stopPropagation()` 和 `e.preventDefault()` 在任何浏览器里都能工作。
*   **性能优化**：
    *   React 里的事件对象可能被复用（虽然 React 17 之后取消了对象池复用机制，但其轻量化的设计依然是为了性能）。

#### 如何获取原生事件？
如果你真的需要底层原生的 DOM 事件对象，可以通过 `e.nativeEvent` 属性获取。

```jsx
const handleClick = (e) => {
  console.log(e);             // React 的 SyntheticEvent
  console.log(e.nativeEvent); // 浏览器的原生 PointerEvent / MouseEvent
}
```

---

### 2. 事件委托 (Event Delegation) —— “中央调度中心”

这是 React 事件系统最精妙的地方。

#### 原生 DOM 的痛点
假设你有一个列表 `<ul>`，里面有 1000 个 `<li>`，每个 `<li>` 都要能点击。
*   **原生做法**：你可能会给这 1000 个 `li` 每一个都 `addEventListener`。
*   **后果**：内存里创建了 1000 个监听器对象，浏览器内存爆炸，页面变卡。

#### React 的做法 (顶层代理)
当你写 `<button onClick={...}>` 时，React **并没有** 真的在这个 button 元素上绑定一个 `click` 监听器。

1.  **全局单一监听**：React 启动时，只会在**根节点**（React 17+ 是挂载的 `root` 节点，React 16 是 `document`）上绑定**一个**原生的 `click` 监听器。
2.  **冒泡捕获**：当你点击按钮时，浏览器的原生事件会冒泡到根节点。
3.  **中央调度**：React 的全局监听器捕获到这个事件。
4.  **目标匹配**：React 查看 `e.target`（你实际点击的元素），然后根据它的内部组件树（Virtual DOM），找到这个元素对应的组件，看看它有没有写 `onClick` 属性。
5.  **手动触发**：如果有，React 就把这个事件包装成 `SyntheticEvent`，然后**手动调用**你写的那个回调函数。

**总结：React 实现了一套自己的“事件冒泡”机制，模拟了浏览器的行为。**

---

### 3. React 17/18 的重大变革

了解这一点对解决古怪 Bug 很有用。

*   **React 16 及以前**：事件委托挂载在 `document` 上。
    *   *坑*：如果你在一个页面里同时用了 jQuery 和 React，或者用了微前端（多个 React App），`e.stopPropagation()` 可能拦不住冒泡到 document 的事件，导致多个 App 互相打架。
*   **React 17+**：事件委托挂载在 **React 渲染的根容器**（比如 `<div id="root">`）上。
    *   *优势*：这让 React 应用更容易嵌入到其他系统中，同时也修复了微前端场景下的很多冒泡问题。

---

### 4. 只有 React 里的 "onChange" 是个特例

这是一个经典的“修正设计”。

*   **原生 HTML**：`<input>` 的 `onchange` 事件只有在输入框**失去焦点 (Blur)** 且内容改变时才会触发。在你打字的过程中是不触发的。
*   **React**：React 觉得这不合理。React 的 `onChange` 实际上对应的是原生的 `input` 事件。**只要你敲一个字，它就触发一次。**
    *   这使得实现“受控组件”（输入框内容实时同步到 State）变得非常自然。

---

### 5. 总结：React 事件封装图解

想象一下这个流程：

1.  **用户**：点击了 `<button>提交</button>`。
2.  **浏览器**：创建一个原生 `ClickEvent`，从 button 冒泡到 `div#root`。
3.  **React (在 root 监听)**：拦截到这个原生事件。
4.  **React (生成)**：`new SyntheticEvent(nativeEvent)`。
5.  **React (调度)**：查找 Virtual DOM，发现这个 button 组件有一个 `onClick` 属性指向了 `handleSubmit` 函数。
6.  **React (执行)**：`handleSubmit(syntheticEvent)`。

JS原生事件和React合成事件的执行时机不同：
- **原生 JS**：
    - 浏览器触发事件 -> 立即执行你的回调。
- **React**：
    - 浏览器触发事件 -> 冒泡到 Root -> React 捕获 -> **包装成合成事件** -> **模拟组件树冒泡** -> 执行你的回调。
    - 这意味着，如果你混用原生事件和 React 事件，**原生事件通常会先于 React 事件执行**（因为 React 事件要等冒泡到顶层才开始分发）。

### 对开发者的启示 (Takeaway)

1.  **不用担心内存泄漏**：在 React 里给 1 万个元素加 `onClick` 是安全的，因为实际上 DOM 上只有一个监听器。
2.  **遵循 React 写法**：用 `onClick` (驼峰) 而不是 `onclick`。
3.  **不要混用**：尽量不要在 React 组件里混用 `document.addEventListener`，除非你非常清楚 React 的事件流机制，否则容易出现“为什么 `stopPropagation` 不生效”的灵异现象。