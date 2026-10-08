---
type: entity
title: LlamaIndex
tags: [rag, 框架, 代码索引]
related: [code-insight, 向量检索rag, langchain]
created: 2026-06-12
updated: 2026-10-08
sources: ["[202601061754]AI实践CodeInsight代码搜索定位的实践分享.html", "[202605181736]RAG全链路技术详解.html"]
---
# LlamaIndex

RAG（检索增强生成）基础框架。[[有赞技术中台]]在构建 [[code-insight|Code Insight]] 系统时选用 LlamaIndex 而非 LangChain，理由为：

1. **轻量化封装**：相比 LangChain 的较重封装，LlamaIndex 更加轻量灵活
2. **多模型厂商对接**：原生支持对接多家模型厂商的 API

## 应用案例

- [[code-insight|Code Insight]] 的 RAG 路径基础框架，与自选嵌入模型、[[deepseek-v3]]、HNSW 向量检索算法配合使用

## ## 关联证据

- 《RAG全链路技术详解》（大淘宝技术·天猫品牌行业架构，2026-05）将 LlamaIndex 与 Microsoft GraphRAG、LightRAG 并列为 GraphRAG 代表性框架（仅点名未展开）。
