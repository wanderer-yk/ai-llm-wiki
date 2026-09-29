---
type: concept
title: RL 驱动搜索策略
tags: [reinforcement-learning, search-strategy, agent, deep-research]
related: [deep-research, deepsearch, react-agent]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# RL 驱动搜索策略

RL 驱动搜索策略是 [[deep-research|Deep Research]] 的核心技术特征之一：通过强化学习（RL）而非提示词工程，让 Agent 主动认知何时搜索、何时推理。

## 与提示词工程的区别

传统 Agent 依赖精心设计的提示词来指导搜索行为（如"当你不确定时请搜索"），这种方式脆弱且泛化能力差。RL 驱动搜索策略通过强化学习训练，让模型真正学会了搜索策略本身——不需要外部提示，模型内部已经形成了搜索与推理的自主决策能力。

## 在 DeepSearch 中的体现

[[deepsearch|DeepSearch]] 引擎的运行模式为 `<think>`→`<search>`→`<information>`→`<think>`→`<answer>`，这一循环中的搜索时机判断、搜索路径规划、推理与搜索的切换都是由 RL 训练获得的策略驱动的。

## 意义

RL 驱动搜索策略是 Deep Research 超越普通 RAG 和普通 [[react-agent|ReAct Agent]] 的关键——它标志着搜索能力从"外部规则约束"进化为"模型内生智能"。