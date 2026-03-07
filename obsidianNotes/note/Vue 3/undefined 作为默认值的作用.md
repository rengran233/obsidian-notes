在 `oc-banner` 组件中，将 `expanded` prop 的初始值定义为 `undefined` 是为了实现 **受控（Controlled）与非受控（Uncontrolled）混合模式** 的一种常见设计模式。

这种设计让组件既能“开箱即用”，又能支持“外部完全控制”。

### 具体作用解析

**1. 区分“用户是否传参”**
在 Vue 中，`boolean` 类型的 prop 默认值通常是 `false`。如果我们将其设为 `false`，组件内部就无法区分“用户显式传入了 `false`”还是“用户根本没传这个 prop”。
通过设为 `undefined`，我们可以明确判断出意图：
*   **`undefined`**: 用户没传，使用组件内部逻辑（非受控）。
*   **`true` / `false`**: 用户传了值，完全听用户的（受控）。

**2. 实现混合状态逻辑 (`isExpanded`)**
请看代码中的这一段逻辑：

```typescript
const isExpanded = computed({
  // 核心判断逻辑：
  // 如果 props.expanded 不为 undefined，说明父组件想控制状态，优先使用 props.expanded
  // 否则，使用组件内部维护的 innerExpanded
  get: () => (props.expanded !== undefined ? props.expanded : innerExpanded.value),
  
  set: (val: boolean) => {
    innerExpanded.value = val;
    // 无论哪种模式，都发出事件，保证 v-model 和事件监听都能正常工作
    emit("update:expanded", val);
  },
});
```

### 两种使用场景对比

**场景 A：非受控模式（开箱即用）**
用户不需要维护状态，组件自己管自己。
```html
<!-- 用户只写了 expandable，没传 expanded -->
<oc-banner expandable />
```
*   `props.expanded` 是 `undefined`。
*   组件读取 `innerExpanded`（默认为 `true` 或 `false`）。
*   点击按钮时，组件修改 `innerExpanded`，界面自动更新。

**场景 B：受控模式（外部控制）**
用户希望完全控制展开/收起（例如：点击外部按钮控制 Banner，或者多个 Banner 联动）。
```html
<!-- 用户传了 expanded -->
<oc-banner expandable :expanded="parentState" />
```
*   `props.expanded` 是 `true` 或 `false`。
*   组件忽略 `innerExpanded`，直接展示 `parentState` 的值。
*   点击按钮时，组件触发 `update:expanded` 事件，父组件更新 `parentState`，数据流回 Banner 更新视图。

### 总结
定义为 `undefined` 是为了**让组件根据是否传参自动降级**：
*   **有传参** -> 听父组件的（受控）。
*   **没传参** -> 自己管理状态（非受控）。

这是一种增强组件灵活性和易用性的高级技巧。