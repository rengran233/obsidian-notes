在 React 中，“透传”通常指**Props Drilling**（属性钻取）或**Spread Attributes**（属性展开），与 Vue 的“自动透传”机制不同，React 的透传是**显式**且**手动**的。

### 1. 什么是 React 的透传？

通常指的是将父组件接收到的未知 props，原封不动地传递给子组件或 DOM 元素。

**核心写法**：使用 ES6 的剩余参数 (`...rest`) 和 展开语法 (`{...rest}`)。

#### 示例：封装一个 Input 组件
我们希望这个 `MyInput` 组件能接收所有标准的 `<input>` 属性（如 `placeholder`, `type`, `onChange`, `className`），而不需要手动一一声明。

```jsx
// 1. 解构出组件自己需要的 props (如 label)
// 2. 将剩下的所有 props 收集到 `rest` 变量中
function MyInput({ label, ...rest }) {
  return (
    <div className="input-wrapper">
      <label>{label}</label>
      {/* 3. 将 rest 中的属性展开，透传给原生 input */}
      <input {...rest} />
    </div>
  );
}

// 使用时
<MyInput 
  label="Username" 
  placeholder="Enter name"  // 透传
  onChange={handleChange}   // 透传
  className="my-class"      // 透传
/>
```

### 2. React 透传 vs Vue 透传

| 特性 | React | Vue |
| :--- | :--- | :--- |
| **机制** | **手动** (显式编码) | **自动** (隐式行为) |
| **写法** | `{...rest}` | 无需写代码 (默认行为) 或 `v-bind="$attrs"` |
| **灵活性** | 极高。你可以把 `rest` 传给任何一个子元素，或者拆分传递。 | 默认只能传给根元素。传给内部元素需要禁用 `inheritAttrs` 并配合 `$attrs`。 |
| **Class 合并** | **不自动合并**。`<div className={rest.className} />` 会覆盖原有的 class，除非使用库 (如 `clsx`) 手动拼接。 | **自动合并**。根元素的 class 会和透传来的 class 智能叠加。 |
| **事件处理** | `onClick` 只是一个普通的 prop，透传就是把函数传下去。 | 原生事件监听器会自动绑定到根元素。 |

### 3. 常见的“透传”相关概念

#### A. 属性钻取 (Props Drilling)
指数据通过多个中间层组件一层层传递。
```jsx
<GrandParent>
  <Parent prop={data}>  {/* 只是路过 */}
    <Child prop={data} />
  </Parent>
</GrandParent>
```
**解决方案**: Context API (`useContext`) 或状态管理库 (Redux/Zustand)。

#### B. Context 透传
使用 Context 让深层组件直接获取数据，跳过中间传递。
```jsx
const ThemeContext = React.createContext('light');

function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

function ThemedButton() {
  const theme = useContext(ThemeContext); // 直接获取，无需 props 透传
  return <button className={theme} />;
}
```

### 总结
当你听到 React 的“透传”时，90% 的情况是在说 **`{...props}` 写法**。
它赋予了高阶组件或封装组件强大的灵活性，让使用者感觉像是在操作原生 DOM 一样。