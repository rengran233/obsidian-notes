# Tags（仓库标签索引）

本文件用于记录本 Obsidian 仓库中**已经出现**的 tag，以及基于当前目录/主题结构**建议补充**的 tag，用于后续整理。

## 现有 tags（已出现）

> 统计口径：扫描所有 `*.md`；
> - 头部 YAML frontmatter：`---` 内的 `tags:`
> - 正文行内：`#tag`（排除代码块/行内代码/类似 `#include` 等非标签用法）

### 头部（YAML frontmatter）

- `Vue`（出现 17 次）
- `Java`（出现 1 次）
- `项目结构`（出现 2 次）

### 正文行内（`#tag`）

- （当前扫描结果为 0）

## 建议新增的 tags（用于整理仓库）

下面这些 tag 建议来自当前仓库的目录与主题分布（例如 `C++相关/`、`前端/React/`、`数学/` 等）。它们目前**不一定存在**，用于后续逐步给相关笔记补齐标签。

### 按领域/学科

- `cpp`、`stl`、`oop`、`template`、`exception`（对应 `C++相关/` 下的主题）
- `java`、`juc`、`exception`（对应 `Java/`，并与现有 `Java` 做大小写统一时可选）
- `frontend`、`css`、`javascript`、`typescript`、`react`、`vue`（对应 `前端/` 与 `Vue 3/`）
- `math`、`calculus`、`discrete-math`、`probability`（对应 `数学/` 与其子目录）
- `physics`（对应 `大学物理/`）
- `db`、`sql`、`miniob`（对应 `数据库/`）
- `ds`、`algo`、`leetcode`、`luogu`（对应 `数据结构和算法/` 与题库目录）
- `computer-organization`、`verilog`、`cache`（对应 `计组/`）

### 按课程/来源

- `course/cs61a`、`course/cs61b`、`course/stat110`、`course/missing-semester`（对应 `公开课/`）

### 按用途/笔记类型

- `cheatsheet`（例如公式速查类笔记）
- `howto`（工具/环境/操作步骤，例如 `杂记/小工具用法/`）
- `debug`、`pitfall`、`faq`（错误记录、踩坑总结、问答式笔记）
- `todo`（任务清单类，如 `TODOS.md`、日程/代办相关）
- `journal`（日记类，对应 `日志/`）

## 建议的 tag 书写方式（与本仓库现状兼容）

- 建议优先使用**头部 YAML**：
  - `---`\n`tags:`\n`  - vue`\n`---`
- 若需要层级，可用 `/`：例如 `course/cs61a`、`math/calculus`。
- 目前仓库里同时存在 `Java`（首字母大写）与 `vue`（小写）；后续新增 tag 时建议选定一种大小写风格并保持一致。
