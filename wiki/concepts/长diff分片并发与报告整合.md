---
type: concept
title: 长Diff分片并发与报告整合
created: 2026-06-25
updated: 2026-06-25
tags: [diff, chunking, concurrency, token-management, report-aggregation, 货拉拉]
related: [业务级code-review, diff预处理流水线, rag经验召回引擎]
sources: ["[202602271400]如何用AI做业务级CodeReview.html"]
---
# 长Diff分片并发与报告整合

## 定义

长 Diff 分片并发与报告整合是 [[货拉拉大前端]] AI Code Review 系统的工程优化方案，针对大规模 Diff（如跨多文件、多功能模块的合并请求）场景设计，包含四项协同优化策略：Token 安全余量分片、优先级权重计算、并发 API 调用和多 Chunk 报告整合。

## Token 安全余量分片

### 策略

系统对预处理后的 Diff 进行粗略 Token 估算，当累计 Token 数超过 **30000** 时自动创建新的 Chunk。每个 Chunk 包含：

- 文件列表
- 变更元信息
- 平均权重（由该 Chunk 内所有文件变更的权重取平均值得到）

### 设计考量

30000 tokens 的阈值设定考虑了"安全余量"——既保证单个 Chunk 不会超出模型的上下文窗口限制，又为 Prompt 中注入的语义简要和 RAG 召回知识预留了足够空间。

## 优先级权重计算

### 机制

`DiffProcessor` 类提供了 `calculateWeight` 方法，根据文件路径和重要程度计算每个变更的权重。核心机制包括：

- **coreDirs 路径配置**：预先配置核心业务目录路径列表
- **权重分配**：位于 coreDirs 中的文件变更获得更高权重，优先处理

### 目的

确保核心业务逻辑的变更不会被淹没在大量非核心变更中，避免因 Chunk 边界划分导致关键审查遗漏。

## 并发 API 调用

### 实现

```javascript
Promise.all(chunks.map(chunk => reviewChunk(chunk)))
```

所有 Chunk 通过 `Promise.all` 并行提交给 LLM API 进行审查，而非串行处理。

### 效果

显著缩短大规模 Diff 的审查耗时，使系统能够在可接受时间内完成对大型合并请求的完整审查。

## 多 Chunk 报告整合

### 机制

```
reportStore（缓存各 Chunk 报告）
    → isAllChunksDone（校验所有 Chunk 是否完成）
    → 触发最终报告聚合推送
```

每个 Chunk 的审查结果首先缓存到 `reportStore` 中。系统通过 `isAllChunksDone` 方法校验所有 Chunk 是否均已完成，当全部完成后触发最终报告的聚合和推送。

### 设计考量

报告整合机制确保开发者收到的是一份完整的审查报告，而非多个碎片化的 Chunk 报告。聚合过程可能包含去重、排序和优先级标注等后处理步骤。

## 与其他分片/并发方案的对比

- 与 [[追加式上下文]] 的差异：追加式上下文是对话历史无限增长的问题描述，货拉拉的分片策略是对输入 Diff 的主动切分管理
- 与 [[上下文压缩策略]] 的关系：分片是一种"预防性"上下文管理——在 Token 溢出之前就主动切分，而非事后压缩
- 与 [[文档分块参数调优]] 的对比：有赞针对 RAG 文档的分块使用 chunk_size=600/chunk_overlap=100，货拉拉针对 Diff 的分块使用 30000 tokens 阈值，两者分块对象和目标不同