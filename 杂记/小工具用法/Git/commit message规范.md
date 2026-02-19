在软件开发中，目前最主流、最被广泛认可的 Commit Message 规范是 **Conventional Commits**（约定式提交），它主要源于 Angular 团队的规范。

遵循这些规范不仅能让提交历史整洁易读，还能配合工具自动生成 Changelog（更新日志）或触发版本发布。

以下是按照**使用频率**和**重要性**分类的 Commit 类型详解：

### 1. 核心业务类型（最常用 ⭐⭐⭐⭐⭐）

这两类直接对应软件的版本号变更（Semantic Versioning），是产品经理和用户最关心的。

- **`feat`** (Feature)
    
    - **含义**：新增功能。
        
    - **场景**：开发了一个新接口、增加了一个按钮、写了一个新页面。
        
    - **对应版本**：通常对应 **Minor** 版本号升级（例如 1.1.0 -> 1.2.0）。
        
    - _例子_：`feat: add user login via google`
        
- **`fix`** (Bug Fix)
    
    - **含义**：修复 Bug。
        
    - **场景**：修复了空指针报错、修复了样式错乱、修复了逻辑漏洞。
        
    - **对应版本**：通常对应 **Patch** 版本号升级（例如 1.1.0 -> 1.1.1）。
        
    - _例子_：`fix: resolve memory leak in image processing`
        

---

### 2. 代码维护与质量类型（常用 ⭐⭐⭐⭐）

这两类修改了代码，但**不影响**最终用户使用的功能逻辑。

- **`refactor`** (Refactoring)
    
    - **含义**：代码重构。
        
    - **关键点**：即不是新增功能，也不是修改 Bug 的代码变动。
        
    - **场景**：重命名变量、抽离公共函数、优化代码结构、简化逻辑。
        
    - _例子_：`refactor: simplify user authentication logic`
        
- **`perf`** (Performance)
    
    - **含义**：性能优化。
        
    - **场景**：提升页面加载速度、减少内存占用、优化 SQL 查询。
        
    - _例子_：`perf: decrease initial load time by lazy loading components`
        

---

### 3. 开发辅助类型（常用 ⭐⭐⭐）

这些修改通常不涉及业务逻辑代码（Production Code）。

- **`docs`** (Documentation)
    
    - **含义**：文档变更。
        
    - **场景**：修改 README、添加代码注释、修改 API 文档。
        
    - _例子_：`docs: update installation instructions`
        
- **`style`** (Styles)
    
    - **含义**：代码格式调整（**注意：不是 CSS 样式**）。
        
    - **场景**：空格、缩进、逗号、分号、删除空行等不影响代码运行的变动。（通常由 Prettier/ESLint 自动处理）。
        
    - _例子_：`style: format code with prettier`
        
- **`test`** (Tests)
    
    - **含义**：测试相关。
        
    - **场景**：添加缺失的测试、修复失败的测试用例。
        
    - _例子_：`test: add unit tests for user service`
        

---

### 4. 项目构建与工具类型（偶尔使用 ⭐⭐）

这些主要涉及项目配置和依赖管理。

- **`chore`** (Chores)
    
    - **含义**：杂务/构建过程或辅助工具的变动。
        
    - **场景**：更新 npm 依赖包、修改 `.gitignore`、修改构建脚本（rollup/webpack 配置）。
        
    - _注意_：有些团队不喜欢用 `chore` 这个词，可能会细分为 `build` 或 `ci`。
        
    - _例子_：`chore: update react to version 18`
        
- **`build`**
    
    - **含义**：构建系统或外部依赖项的变更。
        
    - **场景**：Webpack、Gulp、npm 脚本变更。
        
    - _例子_：`build: upgrade webpack config`
        
- **`ci`** (Continuous Integration)
    
    - **含义**：CI 配置文件或脚本的变更。
        
    - **场景**：修改 GitHub Actions、Travis、Jenkins 配置。
        
    - _例子_：`ci: fix github action workflow`
        
- **`revert`**
    
    - **含义**：回退代码。
        
    - **场景**：撤销之前的某次 commit。
        
    - _例子_：`revert: feat: add dark mode`
        

---

### 💡 标准格式模板

一个标准的 Commit Message 应该长这样：

Plaintext

```
<类型>(<范围>): <描述>

<正文> (可选)

<脚注> (可选)
```

**示例：**

Plaintext

```
feat(user): add password reset functionality

Added a new API endpoint to handle password reset requests via email.
Includes validation and token generation.

Closes #123
```

### 📝 总结：我该选哪个？

- 写了新代码给用户用？ -> **`feat`**
    
- 修好了报错？ -> **`fix`**
    
- 整理代码看着顺眼（没改逻辑）？ -> **`style`**
    
- 整理代码逻辑（没改功能）？ -> **`refactor`**
    
- 写了注释或文档？ -> **`docs`**
    
- 改了配置文件或升级了包？ -> **`chore`**