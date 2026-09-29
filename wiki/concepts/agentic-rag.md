---
type: concept
title: Agentic RAG
tags: [rag, agent, 多跳推理, 主动澄清, 规划能力, 未来方向]
related: [graphrag, agent-loop, plan-and-execute模式, 多跳推理, agent框架三要素]
created: 2026-06-25
updated: 2026-06-25
sources: ["[202603181415]从RAG到GraphRAG货拉拉元数据检索应用实践.html"]
---
# Agentic RAG

Agentic RAG 是 [[货拉拉]] [[大数据技术团队]]在 GraphRAG 方案2.0 取得成效后规划的**未来探索方向**：用 Agent 的规划能力替代固定工作流，实现更智能的检索增强生成。

## 核心理念

当前 GraphRAG 方案2.0 虽然引入了知识图谱和混合检索，但检索流程仍然是固定的（[[双路检索上下文|Local/Global 双路检索]]）。Agentic RAG 旨在让 Agent 自主决定检索策略，根据问题复杂度动态调整检索深度和路径。

## 关键能力

### 多跳推理

复杂问题拆分为子问题，进行多轮"检索-验证"循环。例如"哪些司机的订单量在上周环比增长超过20%且评分高于4.5"这样的复合查询，Agent 可以拆解为多个子查询，分别检索后再综合判断。

### 主动澄清

当用户问题描述模糊或信息不足时，Agent 主动向用户提问以澄清意图，而非基于不充分的上下文猜测并可能产生幻觉回答。这与 [[badcase五分类|Badcase类型4-5（幻觉回答）]]的解决直接相关。

## 与固定工作流的区别

| 维度 | 固定工作流（方案2.0） | Agentic RAG |
|------|---------------------|-------------|
| 检索策略 | 预定义双路检索 | Agent 自主决定 |
| 复杂查询 | 单次检索 | 多轮检索-验证循环 |
| 模糊问题 | 尽力回答（可能幻觉） | 主动澄清 |
| 可控性 | 高 | 需额外约束机制 |

## 与 Wiki 已有概念的关联

- 与 [[agent-loop]]关联：Agentic RAG 的核心运行机制是 Agent Loop（推理+工具调用+上下文更新循环）
- 与 [[plan-and-execute模式]]关联：多跳推理的"拆分子问题+逐步执行"正是 Plan-and-Execute 范式的应用
- 与 [[agent框架三要素]]关联：Agentic RAG 需要 LLM Call（推理）+ Tools Call（检索工具）+ Context Engineering（多轮上下文管理）三要素协同
- 与 [[workflow优先于agent]]（有赞）形成张力：有赞主张客服场景优先选择确定性 Workflow，而 Agentic RAG 代表了"从 Workflow 向 Agent 渐进演进"的未来方向

## 开放问题

Agentic RAG 的具体落地计划与时间线尚未明确。在元数据检索场景中，Agent 自主性的引入是否会带来可控性下降和幻觉风险增加，需要进一步验证。