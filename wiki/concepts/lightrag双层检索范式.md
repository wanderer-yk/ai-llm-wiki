---
type: concept
title: LightRAG 双层检索范式
tags: [lightrag, 检索范式, dual-level-retrieval, 知识图谱]
related: [lightrag, lightrag三函数索引构建, graphrag, graphrag与lightrag对比, AI答疑助手, "[202604101635]AI答疑助手优化实践从RAG到LightRAG的全链路升级"]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604101635]AI答疑助手优化实践从RAG到LightRAG的全链路升级.html"]
---
# LightRAG 双层检索范式

[[lightrag|LightRAG]] 的查询引擎设计：每次查询**同时触发低级与高级两层检索、合并去重**后交给模型，保证具体问题与抽象/全局问题均有上下文覆盖。

## 两层机制

- **低级检索**：向量相似度定位具体实体节点 → 提取该节点的 Value 文本+关系描述拼装上下文。≈ GraphRAG Local Search 减去社区遍历，检索路径更短、延迟更低。
- **高级检索**：不检索实体本身，而检索关系边上的**全局主题关键词**（由 P(·) 让 LLM 增强生成，见 [[lightrag三函数索引构建]]），沿边关联实体汇总全局上下文——在无社区摘要的前提下近似 Global Search 的全局视野。

## 实测质量分层（同批 44 篇文档）

- 具体问题：LightRAG ≈ GraphRAG Local Search；
- 全局问题：高级检索逊于 GraphRAG Global Search（无社区摘要加持），但该团队 80%+ 用户问题是具体技术问题，场景够用。

## 开放问题

- CoT 意图识别产生的多组查询如何与"双层同时触发"衔接注入，来源文章未披露；
- "秒级响应"的实测口径（p50/p95？仅具体问题？）不明确。