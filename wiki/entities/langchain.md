---
type: entity
title: LangChain
tags: [llm框架, 编排, rag, agent, memory]
related: [langchain语义标准化, agent编排四等式, agent胶水层, agent开发经验主义, llamaindex]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# LangChain

LangChain 是一个面向大语言模型应用开发的开源框架，提供 Prompt 模板、链式编排、RAG、Memory、Agent 与工具集成等组件，并衍生出图结构编排库 LangGraph（此为独立可验证的公开背景，非本来源内容）。本 wiki 收录的核心证据来自 [[韩春]] 在《业务逻辑的"坍塌"（上）》第 05 节"包罗万象的 LangChain"中给出的独立评价体系。

## 本文的评价体系

- **真正价值在语义标准化**：LangChain 最大的贡献是与模型厂商形成 Message 四角色共识（System / User / Assistant / Tool），解决角色边界模糊、信息来源混乱、无法利用 KV Cache、跨模型格式差异四个问题。详见 [[langchain语义标准化]]。
- **编排能力被高估**：官方组件架构中 Orchestration 仅占很小一块；各编排范式可还原为代码等价四等式（ReAct=while+if 等）。详见 [[agent编排四等式]]。
- **SSH 类比及其不稳固性**：LangChain ≈ Java 早期 SSH（Struts+Spring+Hibernate）——为不确定的早期提供标准流程与组件接口；但此定位随基座模型能力演进不稳固：确定性达阈值后将向完全 ReAct 迁移，注意力机制重大优化后输入处理职责将转移回基础模型。
- **经验参数体系**：第 08 节回顾 ChatOpenAI 参数、Message Role 分类、Memory 逐出算法（LRU/Summary）、RAG chunk_size/overlap 均为经验配置；理解基础原理后可预估参数效果。与 [[agent开发经验主义]] 互证。
- **Skill 观察**：Skill 未做模型针对性优化时，A 模型效果好切 B 变差不代表 B 更差——这是模型厂商被迫下场提供 Skill 的原因。与 [[mcp与skills竞争关系论]] 呼应。

## LangGraph（并入记录）

本文将 LangGraph 定义为 "Workflow + StateNode"——本质仍是条件分支，图结构表达更灵活。

## 与本 wiki 其他条目的关系

- 与 [[llamaindex]] 同为 RAG 基础框架，但本来源未做两者直接对比。
- 本文对其"编排价值会随基座成熟而迁移"的判断，与 [[workflow优先于agent]]、"先确定性后自主性"的行业共识构成参照。