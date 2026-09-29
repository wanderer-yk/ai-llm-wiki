---
type: entity
title: LanceDB
tags: [tool, vector-database, rag, embeddings]
related: [search-knowledge, 知识基座-天猫, code-insight]
created: 2026-07-21
updated: 2026-07-21
sources: ["[202603231539]知识基座让AI越用越懂业务的团队经验实践天猫AICoding实践系列.html"]
---
# LanceDB

LanceDB 是一个向量数据库，配合 Embedding 模型实现 RAG 向量检索。

## 在天猫知识基座中的应用

[[知识基座-天猫|天猫知识基座系统]] 当前知识量 < 1000 时使用关键词检索，正在向基于 LanceDB 的 RAG 向量检索迁移。迁移的核心判断为"知识量变带来质变"——向量检索能发现关键词无法匹配的语义关联（如"页面白屏"与"组件渲染失败"）。[[search-knowledge]] MCP Server 检索工具支持基于 LanceDB 的三级分级召回策略。

## 在其他系统中的应用

[[code-insight]]（有赞代码搜索定位系统）也使用 LanceDB 作为向量数据库基础设施，配合 [[llamaindex]] RAG 框架实现代码语义检索。