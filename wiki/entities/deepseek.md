---
type: entity
title: DeepSeek
tags: [llm, 模型提供商, 工具调用]
related: [agent-loop, 腾讯混元]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# DeepSeek

LLM 提供商。在 [[sources/[202604131736]详尽地带你从零开始设计实现一个AIAgent框架|yabohe 的文章]]实践篇中，选用 deepseek-chat 模型作为 [[agent-loop|Agent Loop]] 框架的示例 LLM。

## 选型理由

- 支持 Tool Calls（Function Calling）
- 完全兼容 OpenAI SDK，降低接入复杂度

文章在 [[agent框架三要素]] 的工程变量分析中指出，LLM Call 层变量最小，使用 LiteLLM 等工具已可屏蔽不同 LLM 的 API 差异。