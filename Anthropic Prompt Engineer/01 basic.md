# 01 Basic Prompt Structure

## 核心理念
- 先把调用结构写对，再谈 prompt 效果。
- `messages` 规则和 `system` 位置是最基础的使用前提。

## 教程内容
- 最少要有 `model`、`max_tokens`、`messages`。
- 每条消息都要有 `role` 和 `content`。
- 对话必须从 `user` 开始，`user` / `assistant` 必须交替。
- `system` 独立于 `messages`，用于预设行为和风格。
- 教程专门演示了常见错误格式。

## 练习内容
- `Exercise 1.1 - Counting to Three`：写出合法消息格式，让模型输出 1、2、3。
- `Exercise 1.2 - System Prompt`：只改 `SYSTEM_PROMPT`，让模型像 3 岁小孩一样回答。
