---
type: entity
title: OpenSpec
tags: [spec-driven, ai-coding, 开源方案]
related: [spec-driven-development, specflow, github-spec-kit, bmad-method]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# OpenSpec

**OpenSpec** 是一种轻量级规格驱动开发方案，核心为**原子化变更**（Proposal→Apply 闭环），将大功能拆解为小变更包逐个推进。

## 核心贡献

- **原子化变更**：将复杂功能拆解为独立的小变更单元，每个单元走完整的 Proposal→Apply 闭环
- 轻量级设计，强调敏捷和灵活性

## 对 Specflow 的启发

[[specflow|Specflow]] 吸收了 OpenSpec 的原子化变更思想，体现在 Implement 阶段按 Group 编号顺序原子化编码的设计中。

## 在 Cursor 中的局限

- 心智负担重：需要开发者主动管理多个 Proposal 状态
- 状态易丢失：在 Cursor 的对话式工作流中，Proposal 状态缺乏持久化保障