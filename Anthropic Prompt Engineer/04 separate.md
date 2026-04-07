# 04 Separating Data and Instructions

## 核心理念
- 固定指令和可变输入要拆开，prompt 才能复用。
- 变量边界如果不清楚，模型很容易把数据和指令混在一起。

## 教程内容
- 用变量模板处理重复任务。
- 模板能让使用者只替换输入，不改主指令。
- 输入边界不清时，模型会把额外文本误当正文。
- 推荐用 XML 标签包裹输入，如 `<email>`、`<sentences>`。
- 小格式噪声也会影响理解。

## 练习内容
- `Exercise 4.1 - Haiku Topic`：做一个带 `TOPIC` 变量的俳句模板。
- `Exercise 4.2 - Dog Question with Typos`：只靠 XML 标签修复混乱问题。
- `Exercise 4.3 - Dog Question Part 2`：不加 XML，只删少量词来修复 prompt。
