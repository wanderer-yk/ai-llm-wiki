---
type: entity
title: search-knowledge
tags: [tool, mcp-server, knowledge-retrieval, tmall, alibaba]
related: [知识基座-天猫, lancedb]
created: 2026-07-21
updated: 2026-07-21
sources: ["[202603231539]知识基座让AI越用越懂业务的团队经验实践天猫AICoding实践系列.html"]
---
# search-knowledge

`search-knowledge` 是 [[知识基座-天猫|天猫知识基座系统]] 中 MCP Server 提供的知识检索工具，接受用户问题或错误信息，返回标题/内容/相似度分数。

## 三级分级召回策略

1. **仓库级知识**（scope=repo）：相似度 > 0.8 且 ≥ 3 条则直接返回
2. **业务域级知识**（scope=domain）：合并结果，仓库级优先
3. **全局知识**：放宽过滤条件兜底

## 召回技术演进

当前知识量 < 1000 使用关键词检索；正在向 RAG 向量检索迁移，配合 [[lancedb]] 向量数据库和 Embedding 模型实现语义检索。迁移判断依据为"知识沉淀价值随规模增长加速释放"——向量检索能发现关键词无法匹配的语义关联（如"页面白屏"与"组件渲染失败"）。

## 实战示例

当用户遇到 `getPermissionList is not a function` 报错时，AI 调用 `search-knowledge` 返回 pitfall 类型经验，给出针对性三步排查方案，无需用户重新踩坑。

## 关联工具

- [[knowledge-search-experience]] — 同属知识基座召回体系，支持结合用户实际代码配置做针对性分析