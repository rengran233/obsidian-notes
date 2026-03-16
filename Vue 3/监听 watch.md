在 Vue 3 的 Composition API 中，`watch`（侦听器）是你的**“监控摄像头”**。

当你的组件需要**“当某个数据发生变化时，我就去干点什么事”**（比如：发请求、修改 DOM、同步另一个状态），你就必须用到它。

在修改组件库源码时，`watch` 极其常见，尤其是用于处理**父组件传入的 Props 发生变化时，子组件如何响应**。

这里我为你整理了 `watch` 的核心玩法和实习生最容易踩坑的“雷区”：

---

### 一、 基础语法：你在监控谁？(数据源)

`watch` 的第一个参数是“监控目标”（数据源），第二个参数是“回调函数”。

**最容易踩坑的地方在于：你监控的目标到底是什么类型？**

#### 1. 监控单个 `ref` (最简单)

直接把 `ref` 变量丢进去，不需要加 `.value`。

```javascript
import { ref, watch } from 'vue'

const keyword = ref('')

watch(keyword, (newVal, oldVal) => {
  console.log(`搜索词从 ${oldVal} 变成了 ${newVal}`)
})

```

#### 2. 监控 `reactive` 对象中的某个属性 (💣 实习生高发 Bug)

假设你有一个 `reactive` 对象：

```javascript
const form = reactive({ username: 'admin', age: 20 })

```

如果你想单独监控 `username` 的变化：

* ❌ **错误写法**：`watch(form.username, () => {})`
* *原因：`form.username` 只是一个纯粹的字符串 `'admin'`，它失去了响应式，Vue 根本监控不到一个死数据。*


* ✅ **正确写法（使用 Getter 函数）**：
```javascript
watch(
  () => form.username, // 用箭头函数返回那个具体的属性
  (newVal, oldVal) => {
    console.log('用户名变了', newVal)
  }
)

```



#### 3. 同时监控多个数据 (数组语法)

当其中任何一个数据发生变化时，都会触发回调。

```javascript
watch([keyword, () => form.username], ([newKeyword, newUsername]) => {
  console.log('这俩里面肯定有一个变了！')
})

```

---

### 二、 高级配置：`watch` 的三大法宝 (Options)

`watch` 的第三个参数是一个配置对象，这里面藏着组件库开发的核心技巧。

#### 法宝 1：`immediate: true` (立即执行)

* **默认行为**：`watch` 刚写好的时候不会执行，只有等数据**第一次发生变化**时才执行。
* **痛点**：有时候父组件传进来一个 `prop`，子组件需要在刚渲染（挂载）时就根据这个 `prop` 发一次请求，以后变化了再发。
* **解决**：加上 `immediate: true`，Vue 会在初始化时立刻执行一次回调。

```javascript
watch(
  () => props.userId,
  (newId) => {
    fetchUserData(newId) // 组件刚加载时会执行一次，以后 userId 变了也会执行
  },
  { immediate: true }
)

```

#### 法宝 2：`deep: true` (深度监听)

如果你直接监控一个复杂的对象或数组，默认情况下，**只有替换掉整个对象**才会触发 `watch`。修改对象里面的层级极深的某个属性，是不会触发的。

```javascript
const config = ref({ theme: { color: 'red' } })

watch(config, () => {
  console.log('配置变了')
}, { deep: true }) // 开启深度监听

config.value.theme.color = 'blue' // 这样也能触发监控了！

```

> **⚠️ 性能警告**：深度监听会递归遍历对象的所有属性，在处理超大数据（比如长列表）时极度消耗性能，非必要不开启。

#### 法宝 3：`flush: 'post'` (DOM 更新后执行)

* **默认行为**：`watch` 的回调会在 Vue **更新 DOM 之前**执行。此时你拿到的 DOM 还是旧的。
* **场景**：当你监控了一个数据，这个数据变化会导致页面多出一个 `div`，你需要在这个回调里去获取那个新 `div` 的高度。
* **解决**：使用 `flush: 'post'`。

```javascript
watch(showBox, (newVal) => {
  // 加上 flush: 'post' 后，这行代码执行时，页面上的 box 已经渲染完毕了
  const boxHeight = document.querySelector('.box').offsetHeight
}, { flush: 'post' })

```

---

### 三、 面试必问：`watch` vs `watchEffect` 有什么区别？

Vue 3 推出了一个新的 API 叫 `watchEffect`。它们俩极其相似，但分工明确。

| 特性 | `watch` | `watchEffect` |
| --- | --- | --- |
| **声明方式** | 必须明确指出要监控**谁**。 | **全自动**！回调函数里用到了谁，就自动监控谁。 |
| **首次执行** | 默认不执行（除非配置 `immediate: true`）。 | **默认立即执行一次**（为了收集依赖）。 |
| **新旧值** | 能拿到 `newVal` 和 `oldVal`。 | **拿不到！** 只能拿到最新的结果。 |
| **适用场景** | 需要对比新旧值；需要精确控制只监听某个字段；写业务逻辑。 | 无脑同步状态；处理副作用（比如单纯的发网络请求、绑定事件）。 |

**`watchEffect` 极简示例：**

```javascript
const keyword = ref('apple')
const page = ref(1)

// 不用告诉它你要监控谁，只要 keyword 或 page 变了，它自动会重新执行！
watchEffect(() => {
  fetchData(keyword.value, page.value)
})

```

---

### 四、 组件库实战：内部状态同步 (Sync)

既然你在改组件库，你一定经常看到这种模式：**父组件传入了一个值，子组件不能直接改，必须搞一个内部变量中转。**

假设你正在写一个弹窗组件 `<MyModal :visible="show" />`：

```javascript
<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  visible: Boolean
})
const emit = defineEmits(['update:visible'])

// 1. 子组件内部维护一个私有状态，初始值等于外部传进来的 visible
const internalVisible = ref(props.visible)

// 2. 外部的 prop 变了（比如父组件控制它打开），同步给内部
watch(
  () => props.visible,
  (newVal) => {
    internalVisible.value = newVal
  }
)

// 3. 内部状态变了（比如用户点击了弹窗内部的关闭按钮），通知外部
watch(
  internalVisible,
  (newVal) => {
    emit('update:visible', newVal)
  }
)
</script>

```

*注：这是比较经典的基础写法。在 Vue 3.4+ 中，官方推出了 `defineModel` 宏，把上面这一大坨代码简化成了一行。如果你在比较新的组件库里，可能会看到 `defineModel` 替代了这种繁琐的双向 `watch`。*

**你可以回想一下，在你最近修改的组件代码中，有没有看到 `watch` 函数？它是用来监控 `props` 的变化，还是监控内部某个状态来触发动画/请求的呢？**