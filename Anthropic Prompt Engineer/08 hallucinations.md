# 08 Avoiding Hallucinations

## 核心理念
- 降低幻觉的关键，是允许模型说不知道，或先找证据再回答。
- 长文档问答里，“先证据后结论”尤其有效。

## 教程内容
- 给模型“拒答出口”，可减少硬编答案。
- 长文档里，近似信息很容易把模型带偏。
- 先在草稿区摘录证据，再判断是否足够回答。
- 证据不足时，应该返回 `Unknown`。
- 较低 `temperature` 也有助于减少波动。

## 练习内容
- `Exercise 8.1 - Beyoncé Hallucination`：让模型在不确定时承认不知道，避免编错专辑信息。
- `Exercise 8.2 - Prospectus Hallucination`：要求先找证据，再回答招股书中的增长问题。
