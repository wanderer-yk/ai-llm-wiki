---
type: entity
title: GitHub Spec Kit
tags: [spec-driven, ai-coding, 开源方案]
related: [spec-driven-development, specflow, openspec, bmad-method]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# GitHub Spec Kit

**GitHub Spec Kit** 是一种工业级标准化协作协议，通过 **Constitution（宪章）** 定义技术底线，强调**门控（Gating）** 机制——需求阶段未对齐则阻断后续编码。

## 核心贡献

- **门控机制**：在需求对齐完成前，硬性阻断后续编码阶段
- **Constitution 宪章**：定义不可违反的技术底线和规范约束
- 工业级设计，强调严格的质量门禁

## 对 Specflow 的启发

[[specflow|Specflow]] 吸收了 GitHub Spec Kit 的门控思想，体现在 [[blocker-gate|Blocker Gate]] 的设计中——Specify 细节未澄清或 Plan Block 项未回答则强制停顿，"先想清楚再写清楚"。

## 在 Cursor 中的局限

- 在 Cursor 的对话式工作流中，门控状态缺乏持久化
- 与 Cursor 的 Agentic Workflow 集成摩擦较大