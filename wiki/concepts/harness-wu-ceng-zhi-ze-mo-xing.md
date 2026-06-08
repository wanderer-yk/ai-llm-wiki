---
type: concept
title: Harness五层职责模型
tags: [harness-engineering, 职责分层, 架构]
related: [harness-engineering, harness-wu-yao-su, gong-ju-yu-harness-fen-li-yuan-ze]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# Harness五层职责模型

[[harness-engineering|Harness Engineering]]的核心架构框架，将研发协作中的隐性职责显式分层。核心理念：**工具可替换，但职责位置不可缺位**。

## 五层定义

| 层次 | 典型工具 | 核心职责 |
|------|---------|---------|
| **任务编排层** | Linear/JIRA/PMS | 目标/范围/状态/责任人/反馈 |
| **执行依据层** | Pencil/docs/design/plan | 结构冻结/边界定义/非目标 |
| **状态暴露与验证层** | Storybook/runbook/verify | 运行状态显式化/可评审/可回归 |
| **agent执行层** | Cursor/Claude Code/Codex | 代码生成/修改/验证 |
| **评审收口层** | GitHub/GitLab | CI/评审/合并/留痕 |

## 核心分离原则

- **工具**负责执行能力（可替换：Cursor↔Claude Code↔Codex）
- **harness**负责工程条件（必须稳定：入口/协议/验证/约束/回写）
- 将所有规则绑在某个工具的私有prompt里 → 工具一换，团队就要重新整理协作方式
- harness的工程条件应独立于具体工具

## 与已有Wiki概念的映射

- 任务编排层 → [[harness-wu-yao-su|任务入口]]
- 执行依据层 → [[harness-wu-yao-su|执行依据]]，与[[ssot-dan-wen-dang-ce-lue|SSOT单文档策略]]中的plan.md高度对应
- 状态暴露与验证层 → [[harness-wu-yao-su|验证反馈]]
- agent执行层 → [[cursor]]等工具
- 评审收口层 → [[pre-pr-ji-zhi|Pre-PR预审]]、[[blocker-gate|Blocker Gate]]

## 与Specflow的结构性关联

[[specflow|Specflow]]在五层模型中的定位是**执行依据层**的具体实现——通过plan.md规格契约冻结结构、边界和非目标。两者在同一架构中找到了结构性关联。