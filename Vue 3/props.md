---
tags:
  - Vue
---

以下是关于 **Vue 3 Props (`defineProps`)** 的核心笔记整理，结合 React 视角，方便查阅。

---

# Vue 3 Props 核心笔记 (`<script setup>`)

在 Vue 3 的 `<script setup>` 语法糖中，Props 的定义方式与 React 有所不同，必须**显式声明**。

## 1. 核心概念：`defineProps`

*   **作用**: 声明组件接收的属性（输入接口）。
*   **React 对比**: 等同于函数组件的参数 `function Comp(props) {}`。
*   **特性**:
    *   **编译器宏**: 无需 import，直接使用。
    *   **显式声明**: 未声明的属性会被当作透传属性（Fallthrough Attributes），自动挂载到根节点（如 `class`, `style`, `id`）。

## 2. 定义方式 (TypeScript)

### 方式一：纯类型声明 (推荐)
最简洁，利用 TS 泛型直接定义。

```typescript
<script setup lang="ts">
interface Props {
  title: string;
  count?: number;      // 可选
  tags?: string[];
}

// 此时默认值均为 undefined
const props = defineProps<Props>();

console.log(props.title);
</script>
```

### 方式二：带默认值 (`withDefaults`)
TS 类型声明无法直接写默认值，需配合 `withDefaults` 宏。

```typescript
<script setup lang="ts">
interface Props {
  msg?: string;
  count?: number;
}

// React: ({ msg = 'Hello', count = 0 }) => ...
const props = withDefaults(defineProps<Props>(), {
  msg: 'Hello Vue',
  count: 0
});
</script>
```

### 方式三：运行时声明 (非 TS)
类似于 React 的 `propTypes`，提供运行时的类型检查和验证。

```javascript
const props = defineProps({
  // 基础类型检查
  title: String,
  // 详细定义
  count: {
    type: Number,
    required: true,
    default: 100,
    validator: (value) => value >= 0 // 自定义验证
  }
});
```

## 3. 父子传值写法 (Template)

### 字符串 vs 变量
这是最容易混淆的点。

| 写法 | 解析结果 | React 对比 |
| :--- | :--- | :--- |
| `title="Hello"` | **字符串** "Hello" | `title="Hello"` |
| `:title="Hello"` | **变量** Hello (去找 setup 里的变量) | `title={Hello}` |
| `:count="100"` | **数字** 100 | `count={100}` |
| `count="100"` | **字符串** "100" | `count="100"` |
| `:disabled="false"`| **布尔** false | `disabled={false}` |
| `disabled="false"`| **字符串** "false" (真值!) | **坑！** |

> **口诀**: 传**变量、数字、布尔、数组、对象**，统统加**冒号 (`:`)**。

## 4. 单向数据流 (One-Way Data Flow)

与 React 一样，Vue 的 Props 是**单向向下绑定**的。

*   **原则**: 子组件**不应该**直接修改 `props` 的值。
*   **后果**: 如果修改，Vue 会在控制台发出警告，且父组件重新渲染时会覆盖子组件的修改。
*   **解决方案**:
    1.  **Emit 事件**: 通知父组件修改 (类似于 `props.onChange`)。
    2.  **本地状态**: 将 prop 作为初始值存入 `ref` (如果不需同步回父组件)。

```typescript
// ❌ 错误
props.count++; 

// ✅ 正确 (通知父组件)
emit('update:count', props.count + 1);
```

## 5. 解构问题 (Destructuring)

在 `<script setup>` 中，**不要直接解构 `props`**，因为会**丢失响应性**。

```typescript
// ❌ 失去响应性！title 变成了一个普通字符串常量
const { title } = props; 

// ✅ 保持响应性 (使用 toRefs)
import { toRefs } from 'vue';
const { title } = toRefs(props);

// ✅ 或者直接使用 props.title (推荐)
console.log(props.title);
```

> **注**: Vue 3.3+ 推出了 `defineProps` 解构语法糖（实验性），允许 `const { title } = defineProps(...)` 且保持响应性，但目前标准写法仍建议小心解构时小心。
