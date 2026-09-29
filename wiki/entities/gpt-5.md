---
type: entity
title: GPT-5
tags: [llm, openai, long-output]
related: [上下文腐烂, deep-research]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# GPT-5

GPT-5 是具备更长输出能力的大模型代表。

## 在 Deep Research 语境下的局限

虽然 GPT-5 支持超长上下文输出，但在 [[deep-research|Deep Research]] 场景下，Agent 频繁调用工具进行长时间运行时同样必然遭遇"[[上下文腐烂]]"，导致模型性能不升反降。这一发现表明，仅靠扩大模型上下文窗口无法解决 Agent 长时间运行中的性能衰减问题，需要通过[[双层级agent架构|双层级 Agent 架构]]和[[上下文卸载技术]]等工程化手段加以应对。