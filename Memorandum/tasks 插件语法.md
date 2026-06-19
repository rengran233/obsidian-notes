## 一、 行内书写语法（给任务添加属性）

在 `Tasks 表情格式`下，你可以通过在任务行内添加特定符号和日期来赋予它属性。日期支持 `YYYY-MM-DD` 格式，也支持自然语言（如 `today`, `tomorrow`, `next Friday`）。

| 属性名称 | 符号 | 语法示例 | 实际作用解释 |
| --- | --- | --- | --- |
| **截止日期 (Due)** | 📅 | `- [ ] 交付项目报告 📅 2026-06-20` | **死线**。必须在此日期或之前完成，过期会变红。 |
| **计划日期 (Scheduled)** | ⏳ | `- [ ] 构思报告大纲 ⏳ 2026-06-18` | **打算干活的日子**。在这天之前它不需要出现在你的视野里。 |
| **开始日期 (Start)** | 🛫 | `- [ ] 收集报告素材 🛫 2026-06-16` | **可以开始做的日子**。到达此日期前，该任务在很多过滤器中会被隐藏。 |
| **完成日期 (Done)** | ✅ | `- [ ] 任务已搞定 ✅ 2026-06-16` | 点击打勾时自动生成，记录你实际完成的时间。 |
| **循环周期 (Recurrence)** | 🔁 | `- [ ] 每周例会准备 🔁 every Monday` | 完成后会自动在下一行生成一个相同属性的新任务。 |
| **优先级 (Priority)** | 🔺/🔼/🔽 | `- [ ] 核心紧急任务 🔺` | 共有高 `🔺`、中 `🔼`、低 `🔽` 三种（普通无图标）。 |

> **💡 循环周期常用快捷词 (🔁 后面紧跟)：**
> * `every day` (每天) / `every week` (每周) / `every month` (每月)
> * `every weekday` (每个工作日，周一至周五)
> * `every Tuesday` (每周二) / `every 2 weeks` (每两周)

---

## 二、 查询代码块语法（在 ```tasks 内部使用）

当你需要聚合任务时，可以在代码块中输入以下指令。**每一行代表一个过滤或控制条件**。

### 1. 核心状态过滤 (Status)

* `done` ：只看已完成的任务。
* `not done` ：只看未完成的任务。

### 2. 日期过滤器 (Date Filters)

你可以对 `due`（截止）、`scheduled`（计划）、`start`（开始）、`done`（完成）这四种日期进行极度精准的过滤。把下面命令里的 `due` 换成其他三个词同样适用：

* **绝对时间：**
* `due before 2026-06-20` （在 2026-06-20 之前）
* `due after 2026-06-16` （在 2026-06-16 之后）
* `due on 2026-06-18` （恰好在 2026-06-18 当天）


* **相对时间（最常用，动态更新）：**
* `due today` （今天到期）
* `due tomorrow` （明天到期）
* `due yesterday` （昨天到期）
* `due before today` （**已过期/延误** 的任务）
* `due this week` （本周内到期）
* `due next week` （下周到期）
* `due before in 7 days` （未来 7 天内到期）


* **有无日期：**
* `has due date` （设定了截止日期的任务）
* `no due date` （没设截止日期的任务）



### 3. 文本与标签过滤 (Text & Tag)

* `description includes 关键词` ：任务内容包含某个关键词。
* `description does not include 关键词` ：任务内容不包含某个关键词。
* `tags include #工作` ：包含 `#工作` 标签的任务。
* `tags ooo` ：（支持 `tags do not include` 等，以此类推）。

### 4. 路径与笔记过滤 (Path & File)

* `path includes 文件夹名` ：只搜寻某个特定文件夹下的笔记。
* `filename includes 目标文件名` ：只搜寻特定文件名（如只搜日记 `Daily`）。

### 5. 优先级过滤 (Priority)

* `priority is high` （高优先级 `🔺`）
* `priority is medium` （中优先级 `🔼`）
* `priority is none` （普通无优先级）
* `priority is low` （低优先级 `🔽`）

---

## 三、 高级控制语法（排序、分组与视觉）

这些语法通常放在代码块的最底部，用来调整表格长成什么样。

### 1. 排序控制 (Sort by)

支持在一行里组合排序，用逗号隔开。

* `sort by due` （按截止日期先后排序）
* `sort by priority` （按优先级高低排序）
* `sort by path` （按文件路径排序）
* `sort by description` （按文本首字母排序）
* **反向排序：** 在末尾加 `reverse`（例如：`sort by due reverse` 变为从远到近）。

### 2. 分组聚合 (Group by)

把搜出来的任务分门别类地排好，极度好看！

* `group by filename` （按任务所在的**文件名**分组，适合按项目看任务）
* `group by due` （按截止日期分组，比如分成“今天”、“明天”、“未来”）
* `group by priority` （按优先级分组）
* `group by folder` （按所在文件夹分组）

### 3. 界面美化与隐藏 (Hide)

* `hide filter instructions` （隐藏顶部的过滤解释和搜索框）
* `hide task count` （隐藏底部的任务总数计数）
* `hide backlink` （隐藏任务末尾跳转回原笔记的 `🔗` 箭头）
* `hide due date` / `hide priority` （在渲染列表中隐藏图标本身）
* `short mode` （精简模式，隐藏所有日期汉字/英文，只留小图标，极省空间）

---

## 🛠️ 终极合体演练（复制即用）

把上面学到的拼起来，就是一个非常完美的“高管视角的周报/看板”：

```tasks
# 1. 过滤：找出未完成，且这周内必须交差的文件
not done
due before in 7 days
has due date

# 2. 分组与排序：先按优先级分类，同一类里按死线谁先到排队
group by priority
sort by due

# 3. 界面视觉美化
short mode
hide filter instructions

```

```

```