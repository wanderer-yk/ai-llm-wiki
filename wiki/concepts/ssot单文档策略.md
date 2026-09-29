---
type: concept
title: SSOT 单文档策略
tags: [ssot, 文档管理, token优化, 开发效率]
related: [specflow, 单指令状态机, 多agent角色思维隔离]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# SSOT 单文档策略

SSOT（Single Source of Truth，唯一事实来源）单文档策略是 [[specflow|Specflow]] 的四大设计哲学之一，核心做法是将功能拆解、方案与任务状态集中在 **`plan.md`** 单一文件中。

## 核心主张

- AI 编码时无需跨文件检索，降低 **Token 损耗**
- 大幅提升**上下文一致性**，避免多文件间的信息冲突
- 与 [[多agent角色思维隔离]] 配合实现内生审计与校准

## 在 Specflow 中的实现

- `plan.md` 是整个开发流程的核心文档，承载功能拆解、方案设计和任务状态
- 文档存储路径为 `ai-docs/{ID}/`
- 四个核心文件构成完整的文档资产体系：

| 文件 | 路径 | 作用 |
|------|------|------|
| `specify.md` | `ai-docs/{ID}/` | 需求规格 |
| `plan.md` | `ai-docs/{ID}/` | 技术方案与任务拆解 |
| `summary.md` | `ai-docs/{ID}/` | 知识脱水摘要 |
| `ARCHIVE_SUMMARY.md` | 项目根目录 | 全局需求索引（按年/季组织） |