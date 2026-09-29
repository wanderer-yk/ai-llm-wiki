---
type: concept
title: 双层级 Agent 架构
tags: [agent-architecture, planner-workers, context-management, deep-research]
related: [上下文腐烂, 上下文卸载技术, 分级存储架构, 智能剪枝策略, langgraph, deep-research]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html"]
---
# 双层级 Agent 架构

双层级 Agent 架构是 [[deep-research|Deep Research]] 为突破大模型单次 Token 输出限制并缓解[[上下文腐烂]]而提出的分离式 Agent 架构。

## 架构组成

### 监督者层级（Planner / 大脑）

- 需求澄清——理解并明确用户的研究意图
- 子主题分解——将研究目标拆解为可执行的子任务
- 资源分配——为各子任务分配搜索预算和优先级
- 全局整合——最终拼接、润色、消除重复内容和逻辑冲突

### 执行者层级（Workers）

- 搜索——根据 Planner 分配的任务进行信息检索
- 阅读——阅读和理解检索到的文档
- 分析——提取关键信息并进行推理

### 聚合器（Aggregator）

- 最终拼接润色——将各 Workers 的产出整合为完整报告

## 流程控制

双层级架构依托 [[langgraph|LangGraph]] 状态机保障执行有序性，确保监督者与执行者之间的状态同步和流程控制。

## 与上下文管理的配合

双层级架构是缓解[[上下文腐烂]]的框架基础，具体配合以下技术：

- [[上下文卸载技术]]——将关键信息存储在活跃上下文窗口之外
- [[分级存储架构]]——按重要性和使用频率分级保留信息
- [[智能剪枝策略]]——小模型预先提取核心信息
- [[增量生成机制]]——按序逐步生成长篇报告各部分