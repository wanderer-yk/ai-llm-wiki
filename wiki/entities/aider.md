---
type: entity
title: Aider
tags: [ai编码工具, 代码索引, repo-map]
related: [cursor, claude-code, 代码索引]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202601061754]AI实践CodeInsight代码搜索定位的实践分享.html"]
---
# Aider

AI 编码工具，采用 [[repo-map方案|repo-map]] 代码索引方案。

## 业界对比定位

在[[有赞技术中台]]的业界对比中，Aider 与 [[cursor]]、[[claude-code]] 并列为三种主流代码索引方案：

| 维度 | [[cursor]] | [[claude-code]] | Aider |
|------|-----------|----------------|-------|
| 索引策略 | RAG + Merkle Tree | 纯 grep | repo-map |
| 优点 | 语义化分块，检索精准 | 实现简单 | 早期有启发价值 |
| 缺点 | 前期索引成本高 | Token消耗多，上下文冗余 | 大仓库处理不足，效果不稳定 |

## 评价

- 早期对代码索引领域有启发价值
- 对大仓库处理能力不足
- 效果稳定性欠佳
