---
type: concept
title: Blocker Gate（逻辑阻断门控）
tags: [门控, 流程约束, specflow]
related: [specflow, spec-driven-development, men-kong-ji-zhi]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# Blocker Gate（逻辑阻断门控）

**Blocker Gate** 是 [[specflow|Specflow]] 区别于社区方案的核心纪律机制。其核心理念是"先想清楚再写清楚"——在前置阶段未达标时，硬性阻断后续阶段的推进。

## 判定标准

Blocker Gate 的具体判定逻辑为：

1. **Specify 阶段**：细节未澄清（`specify.md` 中 `[User]` 区域的问题未全部回答）→ 阻断进入 Plan
2. **Plan 阶段**：`plan.md` 中 `[Block]` 必答阻塞项未回答 → 阻断进入 Implement

## [Block]/[?] 两级问题分级

这是 Blocker Gate 的可操作化实现：

- **`[Block]` 必答阻塞项**：未回答则无法进入开发阶段，强制阻断
- **`[?]` 可选非阻塞项**：未回答不阻断，开发者可选择跳过

## 与社区方案门控的区别

[[github-spec-kit|GitHub Spec Kit]] 的门控机制是 Blocker Gate 的直接灵感来源，但 Specflow 做了关键增强：

- 门控判定基于文件状态（`ai-docs/` 目录下的文件存在性和内容完整性），而非人工判断
- 通过 `[Block]/[?]` 分级实现了结构化的门控标准
- 门控状态随文件自动流转，解决了社区方案中"状态易丢失"的摩擦

## 与其他 Wiki 概念的关联

- [[men-kong-ji-zhi|门控机制]]（GitHub Spec Kit 的核心启示）是 Blocker Gate 的前身概念
- [[pre-pr-ji-zhi|Pre-PR 预审机制]]（美团）与 Blocker Gate 精神一致，都是人工介入的质量关卡
- [[an-quan-tie-lv|安全铁律]]（马上消费）中的"写操作工具执行前强制授权"与 Blocker Gate 的阻断逻辑异曲同工