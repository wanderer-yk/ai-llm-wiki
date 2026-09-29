---
type: entity
title: GitHub Spec Kit
tags: [sdd, 规格驱动开发, 协作协议]
related: [specflow, openspec, bmad-method, 规格驱动ai开发, blocker-gate]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# GitHub Spec Kit

GitHub Spec Kit 是一种工业级、标准化的**规格驱动开发协作协议**，通过 Constitution（宪章）定义技术底线，强调"先规格后任务"的工作模式。

## 核心特点

- **Constitution（宪章）**：定义项目的技术底线和约束
- **门控（Gating）**：在需求阶段未对齐时阻断后续编码

## 对 Specflow 的启发

- **门控理念**被 Specflow 吸收并强化为 [[blocker-gate|Blocker Gate]] 硬性阻断机制
- **"先规格后任务"**的严谨工作模式影响了 Specflow 的 Specify→Plan→Implement 流程设计

## 在 Cursor 中的局限

根据天玑前端团队的调研，Spec Kit 在 Cursor 中同样存在"摩擦力"，导致状态易丢失和心智负担。