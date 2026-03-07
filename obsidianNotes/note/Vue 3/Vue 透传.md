**透传 (Fallthrough Attributes)** 是 Vue 的一个特性，指：**父组件传给子组件，但子组件并没有在 `defineProps` 或 `defineEmits` 中声明的属性**。

这些属性会自动“穿透”子组件的外壳，直接绑定到子组件的**根元素**上。

### 1. 典型的透传属性
*   `class`
*   `style`
*   `id`
*   原生事件监听器 (如 `@click`)
*   `aria-*`, `data-*`

### 2. 举个例子

#### 子组件 (`MyButton.vue`)
```html
<script setup>
// 只声明了 title，没声明 class 或 click
defineProps(['title']);
</script>

<template>
  <!-- 这里的 button 是根元素 -->
  <button class="btn">
    {{ title }}
  </button>
</template>
```

#### 父组件
```html
<template>
  <!-- 传递了 props (title) 和 透传属性 (class, @click) -->
  <MyButton 
    title="Submit" 
    class="large-btn" 
    @click="handleClick" 
    data-testid="submit-btn"
  />
</template>
```

#### 最终渲染结果
Vue 会自动把 `large-btn`, `@click`, `data-testid` 合并到 `<button>` 上：

```html
<button class="btn large-btn" data-testid="submit-btn">
  Submit
</button>
```
*   `class`: 自动合并（`btn` + `large-btn`）。
*   `@click`: 自动绑定到 `<button>` 上，点击按钮就会触发父组件的 `handleClick`。

### 3. React 对比
*   **React**: 不会自动透传。你需要手动把 `restProps` 撒到 DOM 上：
    ```jsx
    function MyButton({ title, ...rest }) {
      return <button {...rest}>{title}</button>;
    }
    ```
*   **Vue**: **自动透传**。如果组件只有一个根节点，你不需要做任何事，Vue 帮你做。

### 4. 如何禁用透传？
如果你不希望这些属性自动加到根元素上（例如你想把 `class` 加到内部某个特定元素，而不是最外层），可以禁用它。

```javascript
<script setup>
defineOptions({
  inheritAttrs: false // 禁用自动透传
})
</script>
```

然后你可以通过 `$attrs` 变量手动把它们绑定到任何你想绑的地方：

```html
<template>
  <div class="wrapper">
    <!-- 把透传进来的属性手动绑给内部的 input -->
    <input v-bind="$attrs" />
  </div>
</template>
```

### 总结
**透传**就是 Vue 帮你省去了 React 中 `...restProps` 的手动传递过程。
*   **没声明的 Props** -> **自动贴到根节点上**。
*   **Class/Style** -> **自动合并**。
*   **事件** -> **自动监听**。