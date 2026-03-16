`setup()` 是 Vue 3 **Composition API (组合式 API)** 的核心入口。

对于 React 开发者来说，`setup()` 函数**就相当于 React 函数组件的函数体**。它是所有逻辑（状态、副作用、计算属性）开始执行的地方。

目前有两种主流写法：
1.  **显式 `setup()` 函数** (基础写法，需手动 return)。
2.  **`<script setup>`** (语法糖，推荐写法，自动 return)。

---

### 1. 显式 `setup()` 函数 (基础版)

这是 Vue 3 最初引入的形式。它是一个显式的组件选项。

#### 执行时机
*   **早于** `created` 钩子。
*   **晚于** `beforeCreate` 钩子。
*   此时组件实例 (`this`) **尚未创建**，所以在 `setup()` 中**无法访问 `this`**。

#### 参数
`setup(props, context)` 接收两个参数：
1.  **`props`**: 响应式的属性对象。**不能解构**（否则会丢失响应性），除非使用 `toRefs`。
2.  **`context`**: 上下文对象，包含 `{ attrs, slots, emit, expose }`。

#### 返回值
返回的对象中的属性/方法，会直接暴露给 `<template>` 使用。

```typescript
import { ref, onMounted } from 'vue';

export default {
  props: {
    title: String
  },
  // 显式定义 setup
  setup(props, { emit }) {
    // 1. 定义状态
    const count = ref(0);

    // 2. 定义方法
    const inc = () => {
      count.value++;
      emit('update', count.value);
    };

    // 3. 生命周期
    onMounted(() => {
      console.log('Mounted!');
      console.log('Prop title:', props.title); // props 是响应式的
    });

    // 4. 必须 return，模板才能用！
    return {
      count,
      inc
    };
  }
};
```

---

### 2. `<script setup>` (语法糖 - 推荐)

这是现在的最佳实践。它本质上是把整个 `<script>` 块的内容都视为 `setup()` 函数的内部代码。

#### 核心优势
1.  **更简洁**: 不需要 `export default`，不需要 `setup() {...}`，不需要 `return`。
2.  **顶层绑定自动暴露**: 任何在顶层声明的变量、函数、import，都可以直接在 `<template>` 中使用。（但是在`script`中仍然需要访问`props`获取）
3.  **更好的 TypeScript 支持**: 使用 `defineProps` 和 `defineEmits` 宏来定义类型。

#### 示例代码
```html
<script lang="ts" setup>
import { ref, onMounted } from 'vue';

// 1. 定义 Props (不需要 import defineProps)
const props = defineProps<{
  title: string
}>();

// 2. 定义 Emits
const emit = defineEmits(['update']);

// 3. 定义状态 (顶层变量直接暴露)
const count = ref(0);

// 4. 定义方法
const inc = () => {
  count.value++;
  emit('update', count.value);
};

// 5. 生命周期
onMounted(() => {
  console.log('Mounted!');
});
</script>

<template>
  <button @click="inc">{{ count }} - {{ props.title }}</button>
</template>
```

---

### 3. React 开发者视角的 `setup()` 映射

| React 概念 | Vue `setup()` / `<script setup>` |
| :--- | :--- |
| **函数组件体** | `setup()` 函数体 |
| **`useState`** | `ref()` 或 `reactive()` |
| **`useEffect`** | `watch()` 或 `watchEffect()` |
| **`useEffect([], ...)`** | `onMounted()` |
| **`useMemo`** | `computed()` |
| **`useCallback`** | 直接定义普通函数即可 (Vue 自动处理依赖缓存，无需手动优化) |
| **`useContext`** | `provide()` / `inject()` |
| **`props`** | `defineProps()` 或 `setup(props)` 参数 |
| **Ref (DOM)** | `const el = ref(null)` + `<div ref="el">` |

### 4. 常见陷阱

1.  **没有 `this`**: 永远不要在 `setup` 里用 `this`。
2.  **Props 解构丢失响应性**:
    *   ❌ `const { title } = props;` (title 变成了普通字符串，不再响应更新)
    *   ✅ `const { title } = toRefs(props);` (title 变成了 ref，保持响应)
    *   ✅ 在 `<script setup>` 中直接用 `props.title`。
3.  **Ref 解包**:
    *   在 `setup` **JS 代码中**，读取 ref 需要 `.value`。
    *   在 **Template 模板中**，读取 ref **不需要** `.value` (Vue 自动解包)。

### 总结
`setup()` 就是 Vue 3 的**逻辑核心**。
如果你写新项目，请直接使用 `<script setup>`，它会让你的代码看起来和 React Functional Component 非常像，而且更简洁（因为不需要手动 return）。