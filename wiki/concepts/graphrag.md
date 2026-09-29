---
type: concept
title: GraphRAG
tags: [rag, 知识图谱, 图算法, 多跳推理, 检索增强生成]
related: [向量检索rag, rag技术瓶颈, lightrag, microsoft-graphrag, pathrag, 元数据图谱化, 离线在线两阶段架构, 双路检索上下文, graphrag三类实体设计]
created: 2026-06-25
updated: 2026-06-25
sources: ["[202603181415]从RAG到GraphRAG货拉拉元数据检索应用实践.html"]
---
# GraphRAG

GraphRAG 是 RAG（Retrieval-Augmented Generation）的进阶架构，在传统 RAG 的向量检索基础上引入**知识图谱**和**图算法**（如社区发现、中心性分析），以增强检索与生成能力。相比传统 RAG，GraphRAG 显著增强了对复杂问题的推理能力、隐藏关联发现能力和回答的系统性。

## 核心架构

GraphRAG 采用 [[离线在线两阶段架构]]：

**离线阶段**：知识抽取+图构建。文本经过 Chunking→Embedding→Vector DB 存储向量索引；同时通过 LLM 进行知识抽取，识别实体和关系，构建实体关系图存入 Graph DB。

**在线阶段**：双路检索+生成。用户问题向量化后，同时执行向量检索和图检索，将检索结果整合为 Prompt 输入 LLM 生成最终回答。

## 多索引结合

GraphRAG 的关键特征是**多索引结合**：图索引（实体关系图谱）、向量索引（文本块 Embedding）、全文索引（BM25 等词频统计）三者协同工作，支持基于图索引的 [[多跳推理]]。

## 三种 Graph-based RAG 范式

当前主流的 Graph-based RAG 方案可分为三种范式：

| 范式 | 核心维度 | 代表 |
|------|----------|------|
| [[microsoft-graphrag]] | 知识挖掘 | 微软开源方案 |
| [[lightrag]] | 轻量化 | 货拉拉最终选型 |
| [[pathrag]] | 路径推理 | — |

## 元数据场景验证

[[货拉拉]] [[大数据技术团队]]在元数据检索场景中完整验证了 GraphRAG 的效果。从 [[naive-rag元数据检索方案|Naive RAG]]（方案1.0，准确率 55%）升级到基于 LightRAG 的 GraphRAG（方案2.0），实现了准确率 78%、召回率 91%、MRR 0.73 的显著提升。

## 核心洞见

货拉拉实践得出的核心结论：**元数据场景 RAG 瓶颈往往不在大模型，而在检索和知识组织；元数据天然具有知识图谱表达方式**。这为 [[元数据图谱化]]概念提供了实证支撑，同时也为 [[rag技术瓶颈]]和 [[向量检索rag]]的局限性分析补充了系统化案例。