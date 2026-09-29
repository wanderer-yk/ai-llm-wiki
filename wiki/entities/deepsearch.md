---
type: entity
title: DeepSearch
tags: [ai-agent, search-engine, react, rl]
related: [deep-research, react-agent]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# DeepSearch

DeepSearch 是 [[deep-research|Deep Research]] 的底层搜索与推理循环引擎，是一个"搜索-阅读-推理"的无限循环机制。

## 运行模式

DeepSearch 的运行循环为：

```
<think> → <search> → <information> → <think> → <answer>
```

该模式基于 [[react-agent|ReAct Agent]] 范式，但关键区别在于其搜索策略不是通过提示词工程实现的，而是通过[[RL驱动搜索策略|强化学习（RL）]]训练而成——模型真正学会了何时搜索、何时推理，并具备自我纠错能力。

## 与传统 RAG 的区别

DeepSearch 超越了普通的 RAG（检索增强生成），因为它具备自主性和[[长链条推理]]能力，不再是被动等待用户给出查询关键词，而是主动规划搜索路径。