- num-info-item中使用了align-items: baseline来使大小不一的文字对齐，这种模式会以最大字号为准，将其他顶部和基线间距离更短的元素强制与最大字号的基线对齐。
- 因此在修改数字字号大小后，item内部的对齐模式发生了变化，和其他item不再能对齐；但因为没有字号不一的场景，该问题可以忽视。
- 不应使用原生的typeof，而是封装好的isNumber等函数
- type.ts中定义组件对外暴露的props和slots，而内部子组件（如num-info-item）的props只被自己使用，可以直接写在文件内
- 使用class-name而不是直接用class


- 父节点报错状态
	- 新增一级节点在error状态时的样式，当设置`status: "error"`时生效
	- 改造`menuList`的计算，在遍历子节点时如果有错误，父节点也显示error状态

- 折叠逻辑调整，提供配置项可控制不折叠
	- `autoCollapse?: boolean`（默认 `true`，是否启用“按屏幕宽度自动折叠”）
	- 继续保留现有`isCollapsed?: boolean`强制折叠

- **实现方式（逻辑）**
  - 把 `resizeSideBar`（`index.vue:383-392`）改为：
    - `const shouldAutoCollapse = props.autoCollapse && width < (props.collapseBreakpoint ?? 1440)`
    - `collapsed.value = props.isCollapsed || shouldAutoCollapse`
  - 监听：
    - `window.resize` 继续用防抖触发 `resizeSideBar`（`index.vue:394-396`, `426-433`）
    - `watch(props.isCollapsed)` 继续保留，并补 `watch(props.autoCollapse)`、`watch(props.collapseBreakpoint)` 触发 `resizeSideBar`，保证配置变更即时生效（当前已有 `watch(props.isCollapsed)`，`index.vue:410-415`）
  - 这样就能做到“完全不折叠”：设置 `:auto-collapse="false"` 且 `:is-collapsed="false"`，无论屏幕多窄都不折叠。

- **用法示例**
  - 禁用自动折叠：`<create-left-menu :auto-collapse="false" :is-collapsed="false" />`
  - 自定义阈值：`<create-left-menu :collapse-breakpoint="1280" />`

如果你希望我把上述两个方案直接落到代码里（新增 props + 改 `menuList/resizeSideBar` + 补一级 error 样式），我可以在 `create-left-menu/index.vue`、`create-left-menu/type.ts`、`create-left-menu/index.less` 上一次性给出可接受的改动 diff。


因为这些 props 的“默认值策略”不一样，取决于它们在组件里是否被当作**必用数据源**、以及是否会被直接遍历/解构使用。

以你标的这段（`promotion-custom-column/index.vue#L133-135`）为例：

- `showBaseDataConfig: undefined`
  - 它是**可选功能开关类配置**：不传就不展示“基础数据”这一块（内部用 `v-if="showBaseDataConfig"`）。
  - 用 `undefined` 能明确表达“未开启该功能”，也便于 `v-if` 直接判断。

- `metricsConfig: () => []`
  - 它是**核心数据源**（指标树），组件内部会做遍历/计算（比如菜单、搜索等）。
  - 默认给空数组可以避免 `undefined` 时大量的判空逻辑，也能保证 `v-for`/`reduce` 这类操作不报错。

- `selectedMetrics: () => ({})`
  - 它同样是**核心数据源**（已选指标），内部会读 `selected/frozen` 并做拼接、长度统计等。
  - 默认给空对象让内部逻辑可以按“无选中项”跑通，不需要先判断 `props.selectedMetrics?....`。

一句话：  
- **功能开关类/可选配置**通常用 `undefined` 表示“没开/没配置”；  
- **会被直接当集合遍历/操作的核心数据**通常给“空集合默认值”（`[]` / `{}`）以减少判空和运行时错误。