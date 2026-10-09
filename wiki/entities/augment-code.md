---
type: entity
title: Augment Code
created: 2026-10-09
updated: 2026-10-09
tags: [代码理解, context-engine, code-graph, 范式三, mcp, Context Engine, 代码检索, 评测]
related: [qodo, deepwiki, cursor, umodel, 代码理解的五种范式, mcp, ai工程量化效果声明追踪]
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# Augment Code

Augment Code 是一家以 **Context Engine（上下文引擎）** 为核心的 AI 编码辅助厂商，在《[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱》一文中被作为"代码理解第三范式——Code Graph + RAG 混合"的代表之一，同时也是该文多处关键论据（尤其是对 [[cursor|Cursor]] 的失败评测）的来源。其厂商身份属可独立核验的公开背景；以下细节均转述自该文。

## 技术能力（据原文记载）

- **跨仓库语义索引**：索引范围含 commit 历史、codebase patterns、外部文档、ticket、tribal knowledge。
- **Context Lineage**（2025 年发布）：索引 commit 历史与 diff 摘要，向"时间维度"延伸；但原文将其归为"部分时间维度"，与 [[umodel|UModel]] 五种 LogSet 构成的完整时间线仍有差距。
- **MCP 开放**：经 [[mcp|MCP]] 协议开放能力（旁证 MCP 生态）；原文称基准测试显示 30-80% 质量提升——基线与任务均未定义、无口径，已记入 [[ai工程量化效果声明追踪]]。

## 对 [[cursor|Cursor]] 的关键评测

Augment Code 评测指出：Cursor 在 50+ 文件的跨文件重构中表现不一致——前 30 个文件改对，后 20 个因上下文窗口溢出错改；评测方法未公开。

- 该评测是原文"CodeIndex 不懂结构、上下文窗口溢出导致重构不一致"论点的关键实证。
- 但它属竞争厂商出具的对比结论，可信度依赖 Augment 的评测透明度；评测方法未给出，已记入 [[ai工程量化效果声明追踪]]。

## 在原文论证中的双重角色

1. **范式三代表**：其 Context Lineage 被原文引为"CodeIndex 流派也在向时间维度演进"的证据。
2. **Cursor 的批评者**：上述评测被原文引为"向量索引做不了结构化推理"的证据。

同时，原文指出范式三的四大边界同样适用于 Augment Code（详见 [[代码理解的五种范式]]）：

- 图限于代码域
- 查询能力受限
- IDE 局部、非团队全局
- 缺时序维度

## 关联

- 范式框架：[[代码理解的五种范式]]
- 同范式代表：[[qodo]]
- 被评测对象：[[cursor]]
- 对照方案：[[umodel]]、[[deepwiki]]