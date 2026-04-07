# 05 Formatting Output and Speaking for Claude

## 核心理念
- 指定输出结构，可以提升可读性、可解析性和稳定性。
- 用 assistant 预填开头，相当于替 Claude 先起笔，约束会更强。

## 教程内容
- 可用 XML 标签包住目标输出，便于提取。
- 可预填如 `<haiku>`，让模型直接续写标签内内容。
- JSON 也能用预填方式约束结构。
- 输入变量和输出格式可以同时结构化。
- 停止序列可用于截断多余收尾。

## 练习内容
- `Exercise 5.1 - Steph Curry GOAT`：只改 `PREFILL`，把回答引向库里是 GOAT。
- `Exercise 5.2 - Two Haikus`：用 XML 明确输出两首关于同一动物的俳句。
- `Exercise 5.3 - Two Haikus, Two Animals`：分别为两个动物输出各一首俳句。
