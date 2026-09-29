---
type: entity
title: OpenSpec
tags: [sdd, 规格驱动开发, 开源工具, fission-ai]
related: [specflow, github-spec-kit, bmad-method, 规格驱动ai开发, ai-native研发模式]
created: 2026-06-12
updated: 2026-09-29
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html", "[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册.html"]
---
# OpenSpec

OpenSpec 是 **Fission AI 开源**的轻量级、面向变更的敏捷**规格驱动开发（SDD）工具**（npm 包 `@fission-ai/openspec`，GitHub `Fission-AI/OpenSpec`，"AI-native system for spec-driven development"，2025-09 首发公立包），核心为 Proposal/Apply 闭环，推崇**原子化变更**理念——将大功能拆解为小变更包。

## 归属定谳（2026-09-29 裁决）

Wiki 曾并存两种表述引发"跨团队归属矛盾"待审项，现考证为**同一个开源工具，两家公司两种使用姿态**：

- **爱奇艺·天玑前端**：作为业界方案**调研对象**——在自研 [[specflow]] 前拆解了 OpenSpec / GitHub Spec Kit / BMAD-METHOD 三种规格驱动方案，最终"不采用任何社区方案"集各家所长自研 Specflow。注意：**plan.md 单文档（[[ssot单文档策略]]）是 Specflow 的特征，不是 OpenSpec 的**，早期条目存在张冠李戴
- **腾讯·binxiong**：**直接采用**该开源工具构建 AI Native 研发流程（全局安装 `@fission-ai/openspec@latest` + `openspec init`），使用其多文件分目录策略（proposal.md + design.md + tasks.md + specs/）

## 对 Specflow 的启发

- **原子化变更**理念被 Specflow 吸收，体现在 Implement 阶段按 Group 顺序逐一原子化开发
- **Proposal/Apply 闭环**的轻量级思路影响了 Specflow 的设计

## 在 Cursor 中的局限

根据天玑前端团队的调研，OpenSpec 在 Cursor 中仍存在"摩擦力"——心智负担重（频繁切换文件）、状态易丢失（多轮对话遗忘 spec.md）。

## 外部验证

- npm：`@fission-ai/openspec`（latest 1.13.2，description: "AI-native system for spec-driven development"）
- GitHub：`Fission-AI/OpenSpec`
