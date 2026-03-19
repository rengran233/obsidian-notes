# Obsidian Vault（obsidian-notes）架构速览

> 本仓库是一个 Obsidian vault：以 Markdown 笔记为主体，辅以 Obsidian 的配置目录 `.obsidian/`（插件、工作区、图谱、日记等设置）。仓库内未发现传统意义上的“应用代码/构建产物/测试框架”。

## 1) 项目概览

- **核心内容**：大量 `*.md` 笔记文件（按主题目录组织），以及一个附件目录 `复制粘贴的图片/`。
- **Obsidian 配置**：`.obsidian/` 存放 vault 的 UI/插件/核心功能开关与部分行为配置。
- **版本管理**：通过 Obsidian 第三方插件 `obsidian-git` 集成 Git，实现自动备份式提交（见 `.obsidian/plugins/obsidian-git/data.json`）。

## 2) 构建与常用命令

- **构建/运行**：仓库内未发现 `package.json`、`Makefile`、CI 工作流等；通常使用 Obsidian 客户端直接打开该目录作为 vault。
- **自动备份提交格式**：`obsidian-git` 的提交信息模板为 `vault backup: {{date}}`（`.obsidian/plugins/obsidian-git/data.json`）。
- **同步策略（插件驱动）**：
  - `syncMethod` 为 `rebase`，且 `pullBeforePush: true`（`.obsidian/plugins/obsidian-git/data.json`）。
  - `autoSaveInterval: 1440`、`autoPullInterval: 1440`（单位为分钟，见 `.obsidian/plugins/obsidian-git/data.json`）。

## 3) “代码风格”与约定

> 该仓库以笔记为主，未发现独立的格式化/命名规范文件（如 lint/formatter 配置）。以下仅记录仓库内实际出现的、对协作有帮助的约定点。

- **笔记格式**：主要为 Markdown。
- **Frontmatter**：存在 YAML frontmatter 的用法示例：`Java/匿名内部类.md` 使用 `tags:` 字段（`Java/匿名内部类.md:1`）。
- **Obsidian 核心插件开关**：核心插件启用/禁用状态在 `.obsidian/core-plugins.json`，例如 `daily-notes: true`、`tag-pane: true`、`slash-command: false`（`.obsidian/core-plugins.json:1`）。

## 4) 测试

- **测试框架/测试命令**：仓库内未发现测试相关配置与可执行入口（例如 `pytest` / `jest` / `go test` / CI）。

## 5) 安全与数据保护

- **Git 凭据交互脚本**：存在 `obsidian-git` 的 askpass 脚本 `/.obsidian/plugins/obsidian-git/obsidian_askpass.sh`，通过环境变量 `OBSIDIAN_GIT_CREDENTIALS_INPUT` 指向的临时文件进行提示与响应交互（`.obsidian/plugins/obsidian-git/obsidian_askpass.sh:3`）。
- **删除确认**：Obsidian 应用层配置 `promptDelete: false`，意味着在客户端内删除文件可能不会弹窗二次确认（`.obsidian/app.json:1`）。

## 6) 配置与环境

### Obsidian 配置目录

- **第三方插件启用列表**：`.obsidian/community-plugins.json` 当前包含：
  - `obsidian-git`
  - `table-editor-obsidian`（Advanced Tables）
  - `calendar`
- **Daily Notes**：日记文件夹配置为 `日志`（`.obsidian/daily-notes.json:1`）。
- **Graph 视图**：图谱展示参数（如 `showTags`、`showAttachments`）在 `.obsidian/graph.json`（`.obsidian/graph.json:1`）。

### 仓库忽略项（Git）

- `.gitignore` 明确忽略与设备/界面布局强相关的文件：
  - `.obsidian/workspace.json`（含注释“记录窗口布局，每台设备不同，必填”）
  - `.obsidian/workspace-mobile.json`
  - `.trash/`

### 插件元信息（已读取）

- `obsidian-git`：版本 `2.37.1`，描述为“Integrate Git version control with automatic backup and other advanced features.”（`.obsidian/plugins/obsidian-git/manifest.json:1`）
- `calendar`：版本 `1.5.10`，描述为“Calendar view of your daily notes”（`.obsidian/plugins/calendar/manifest.json:1`）
- `table-editor-obsidian`（Advanced Tables）：版本 `0.22.1`（`.obsidian/plugins/table-editor-obsidian/manifest.json:1`）

