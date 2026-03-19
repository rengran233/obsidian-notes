---
tags:
  - Vue
---

以下是为你整理的 **Vue 指令 (Directives) 核心笔记**，结合了 React 开发者的视角，方便快速查阅和对比。

---

# Vue 指令核心笔记
 
Vue 指令是带有 `v-` 前缀的特殊属性，用于响应式地操作 DOM。

## 1. 数据绑定 (Binding)

| 指令            | 简写      | 描述                                       | React 对比                            | 示例                                                     |
| :------------ | :------ | :--------------------------------------- | :---------------------------------- | :----------------------------------------------------- |
| **`v-bind`**  | **`:`** | 动态绑定 HTML 属性或组件 Props。引号内解析为 **JS 表达式**。 | `prop={value}`                      | `<img :src="imgUrl" />`<br>`<Comp :isActive="true" />` |
| **`v-model`** | -       | **双向绑定**。通常用于表单元素，自动同步数据和视图。             | `value={x} onChange={e=>setX(...)}` | `<input v-model="text" />`                             |
| **`v-text`**  | -       | 更新元素的 `textContent`。                     | `{text}`                            | `<span v-text="msg"></span>`                           |
| **`v-html`**  | -       | 更新元素的 `innerHTML`。**注意防范 XSS**。          | `dangerouslySetInnerHTML`           | `<div v-html="rawHtml"></div>`                         |

> **关键点**：`:prop="xxx"` 解析变量，`prop="xxx"` 解析字符串。传递数字/布尔值必须用冒号。

## 2. 条件渲染 (Conditional Rendering)

| 指令 | 描述 | 性能特点 | React 对比 |
| :--- | :--- | :--- | :--- |
| **`v-if`** | 如果为假，**DOM 元素完全不存在**。 | 切换开销大，初始渲染开销小。 | `{ cond && <div /> }` |
| **`v-else-if`** | 链式条件判断。 | - | 嵌套三元 / 逻辑与 |
| **`v-else`** | 否则。必须紧跟在 `v-if` 或 `v-else-if` 后面。 | - | - |
| **`v-show`** | 始终渲染 DOM，仅切换 CSS `display: none`。 | **切换开销小**，初始渲染开销大。 | `style={{ display: show ? 'block' : 'none' }}` |

```html
<!-- v-if 示例 -->
<div v-if="score >= 90">优秀</div>
<div v-else-if="score >= 60">及格</div>
<div v-else>不及格</div>

<!-- v-show 示例 (适合频繁切换) -->
<div v-show="isVisible">我还在 DOM 里，只是看不见</div>
```

## 3. 列表渲染 (List Rendering)

| 指令 | 描述 | 关键要求 | React 对比 |
| :--- | :--- | :--- | :--- |
| **`v-for`** | 基于数组或对象渲染列表。语法：`item in list`。 | **必须提供 `:key`** 以优化 Diff 算法。 | `list.map(item => <Comp key={item.id} />)` |

```html
<ul>
  <!-- 遍历数组 -->
  <li v-for="(item, index) in list" :key="item.id">
    {{ index }} - {{ item.name }}
  </li>
  
  <!-- 遍历对象 -->
  <li v-for="(value, key) in myObject" :key="key">
    {{ key }}: {{ value }}
  </li>
</ul>
```

## 4. 事件监听 (Event Handling)

| 指令 | 简写 | 描述 | React 对比 |
| :--- | :--- | :--- | :--- |
| **`v-on`** | **`@`** | 绑定事件监听器。 | `onClick={handler}` |

### 常用修饰符 (Modifiers)
Vue 提供了便捷的修饰符来处理常见 DOM 事件逻辑，React 通常需要在处理函数中手动写。

*   `.stop` -> `e.stopPropagation()` (阻止冒泡)
*   `.prevent` -> `e.preventDefault()` (阻止默认行为)
*   `.self` -> 只有点击元素本身才触发
*   `.enter` -> 只有按下回车键才触发 (按键修饰符)
*   `.native` (Vue 2) -> 监听组件根元素的原生事件 (Vue 3 已移除，自动透传)

```html
<!-- 阻止表单提交 -->
<form @submit.prevent="onSubmit"></form>

<!-- 阻止点击冒泡 -->
<div @click.stop="doSomething"></div>

<!-- 键盘事件 -->
<input @keyup.enter="submit" />
```

## 5. 其他指令

*   **`v-slot`** (简写 **`#`**): 用于组件插槽分发。
    ```html
    <MyComponent>
      <template #header>标题内容</template>
    </MyComponent>
    ```
*   **`v-pre`**: 跳过编译，显示原始 `{{ }}` 标签。
*   **`v-once`**: 只渲染一次，后续数据变化不再更新（静态内容优化）。
*   **`v-cloak`**: 保持在元素上直到 Vue 实例编译结束。配合 CSS `[v-cloak] { display: none }` 防止页面加载时闪现 Mustache 标签。

---

### 快速记忆口诀
*   **冒号 (`:`)** 是给属性传变量的。
*   **艾特 (`@`)** 是用来绑事件的。
*   **`v-model`** 是搞定表单双向绑定的。
*   **`v-if`** 是真的消失，**`v-show`** 是隐身 (`display:none`)。
*   **`v-for`** 必须带 **`:key`**。
