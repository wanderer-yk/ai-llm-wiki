---
type: concept
title: SSOT 单文档策略
tags: [上下文管理, specflow, token优化]
related: [specflow, shang-xia-wen-duan-ceng, dan-zhi-ling-zhuang-tai-ji]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# SSOT 单文档策略

**SSOT（Single Source of Truth）单文档策略** 是 [[specflow|Specflow]] 的核心上下文管理设计。将功能拆解、技术方案与任务状态集中在单一文件 `plan.md` 中，避免 AI 跨文件检索、提升上下文一致性、降低 Token 损耗。

## 设计动机

AI 在跨文件检索时容易出现信息遗漏和上下文稀释，导致[[ai-bian-cheng-huan-jue|幻觉]]问题。SSOT 策略通过将所有关键信息集中在一个文件中，从根本上解决了这一问题。

## 核心收益

1. **避免 AI 跨文件检索**：所有信息在一个文件中，AI 无需在多个文件间跳转
2. **提升上下文一致性**：单一信息源消除了多文件间的信息冲突风险
3. **降低 Token 损耗**：减少重复信息的冗余传输

## 文档资产体系

| 文档 | 路径 | 作用 |
|------|------|------|
| `specify.md` | `ai-docs/{ID}/` | 需求澄清产物 |
| `plan.md` | `ai-docs/{ID}/` | 技术方案 + 任务状态（SSOT 核心） |
| `summary.md` | `ai-docs/{ID}/` | 归档摘要 |
| `ARCHIVE_SUMMARY.md` | 项目根目录 | 全局归档索引 |

## 与 Wiki 其他概念的关联

- [[append-only-context|追加式上下文]]（OpenClaw）是另一种上下文管理策略（全量历史发送），SSOT 选择了"精简集中"而非"全量追加"的路径
- [[shang-xia-wen-duan-ceng|上下文断层]] 是 SSOT 策略要解决的核心问题
- [[context-engineering|上下文工程]]（马上消费）与 SSOT 都致力于精确的上下文交付