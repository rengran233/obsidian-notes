#项目结构

执行`pnpm run story <component>`：根目录`package.json`
### 1. 触发链路
1.  **根目录**: 你在根目录执行 `pnpm run story`。
2.  **`package.json` (根目录)**:
```json
"story": "cd packages/atom && pnpm run story",
```
    这行命令会先进入 `packages/atom` 目录，然后执行该子目录下的 `story` 脚本。
3.  **`packages/atom/package.json`**:
```json
"story": "node .storybook/scripts/dev-empty.js && STORYBOOK_ENV=dev start-storybook -p 1234 --no-manager-cache",
```
    这里才是真正执行的核心逻辑。

### 2. 核心执行步骤详解

当进入 `packages/atom` 后，实际执行了两个步骤（通过 `&&` 连接）：

#### 步骤一：准备环境文件
命令：`node .storybook/scripts/dev-empty.js`

这个脚本的作用是**初始化一些空的测试/覆盖率文件**，防止 Storybook 启动时因为找不到这些文件而报错。
*   它会检查并创建 `.unit_test/results.json` 和 `.unit_test/coverage.json`。
*   如果这些文件不存在，它会写入一个空的 JSON 对象 `{}`。
*   这通常是为了让 Storybook 中某些集成测试结果或代码覆盖率的插件能正常工作，即使还没有运行任何测试。

#### 步骤二：启动 Storybook 服务
命令：`STORYBOOK_ENV=dev start-storybook -p 1234 --no-manager-cache`

*   **`STORYBOOK_ENV=dev`**: 设置环境变量，告诉 Storybook 当前是开发环境。这可能会影响配置文件的加载（例如 `.storybook/main.js` 或 `.storybook/preview.js` 中可能会根据这个变量加载不同的配置）。
*   **`start-storybook`**: 这是 Storybook 的核心启动命令（对应 Storybook v6/v7 早期版本的 CLI，新版通常是 `storybook dev`）。
*   **`-p 1234`**: 指定服务运行在 **1234 端口**。启动成功后，你可以通过 `http://localhost:1234` 访问。
*   **`--no-manager-cache`**: 禁用 Storybook 管理界面的缓存。这在开发组件库时很有用，可以确保你修改了 Storybook 的配置或 UI 后，刷新页面能立即生效，而不会被旧缓存干扰。

### 总结

当你运行 `pnpm run story`：
1.  它会**自动跳转**到 `packages/atom` 目录。
2.  它会**自动创建**必要的空数据文件（避免报错）。
3.  它会在 **1234 端口** 启动一个 Storybook 开发服务器。

**预期结果**：
终端会输出 Storybook 的启动日志，最终提示你访问 `http://localhost:1234`。你可以在浏览器中打开这个地址，预览和调试 `oc-button` 等组件。