#项目结构 

### 1. 入口脚本分析
根据 `package.json`，`pnpm run dockit` 执行的是 `node ./script/run-dockit.js`。

### 2. `script/run-dockit.js` 执行流程

1.  **解析参数**:
    ```javascript
    const argvList = process.argv;
    const comName = argvList.slice(2, argvList.length);
    ```
    它会获取你传给命令的所有参数。例如你运行 `pnpm run dockit oc-button`，那么 `comName` 就是 `['oc-button']`。

2.  **写入配置文件**:
    ```javascript
    const str = `module.exports = ${JSON.stringify(comName)};`;
    fs.writeFileSync(path.resolve(__dirname, '../apps/dockit/run-component-list.js'), str);
    ```
    它会将你要调试的组件列表写入到 `apps/dockit/run-component-list.js` 文件中。这个文件很可能是 dockit 应用的运行时配置，告诉 dockit **只加载并展示你指定的组件**，从而加快启动速度。

3.  **构建依赖 (Vue 组件库)**:
    ```javascript
    execSync("emo exec 'pnpm run build_entry' --filter=@ocean-uikit/vue3", { stdio: 'inherit' });
    ```
    它会调用 `emo`（看起来是一个 Monorepo 管理工具，类似于 Lerna 或 Turbo）来执行 `@ocean-uikit/vue3` 包的 `build_entry` 脚本。这确保了组件库的代码是最新的，并且已经构建好入口文件。

4.  **启动 Dockit 应用**:
    ```javascript
    execSync(`emo start @ocean-uikit-dockit --no-build-deps`, { stdio: 'inherit' });
    ```
    最后，它启动 `@ocean-uikit-dockit` 这个应用（位于 `apps/dockit` 目录）。由于上一步已经构建了依赖，这里使用了 `--no-build-deps` 来跳过重复构建。

### 总结

当你执行 `pnpm run dockit oc-button`：
1.  系统记录下你想调试 `oc-button`。
2.  系统会重新构建一次 `atom` (Vue 组件库) 的入口文件。
3.  系统启动 `apps/dockit` 应用，并根据配置只展示 `oc-button` 的文档和 Demo。

这通常比启动完整的文档站点更快，适合专注于开发某个具体组件。