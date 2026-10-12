---
type: concept
title: Transformer历史阶段性
tags: [transformer, llm, 学习方法]
related: [agent开发经验主义, langchain, 上下文压缩策略, 文档分块参数调优]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# Transformer 历史阶段性

[[韩春]] 第 08 节开头的学习论观点：**Transformer 不是 LLM 浪潮的起点，只是历史演进的一个阶段**；从 Tokenization/Embedding/Attention 解构式入门会误导初学者以为 Transformer 是终态。

## 四个潜在常识问题（列出后 HTML 截断，展开回答缺失）

1. 为何要区分 System/User Prompt？
2. 参数量与效果的关系？
3. 强逻辑长文档选 Chunk 还是 Summary？
4. Multi-Agent 职责拆分依据（对照微服务 DDD）？

## 关联

- 该节是全文的经验主义回顾：理解基础原理后能预估 LangChain 参数（ChatOpenAI 参数、Memory 逐出、chunk_size/overlap）的效果——与 [[agent开发经验主义]] 的缓解之道互证。
- 问题 3（Chunk vs Summary）与 [[上下文压缩策略]]、[[文档分块参数调优]] 直接相关；问题 1 与 [[langchain语义标准化]]、[[推理成本博弈论]] 相关。
- 四问的展开回答可能在系列"下篇"，待确认。