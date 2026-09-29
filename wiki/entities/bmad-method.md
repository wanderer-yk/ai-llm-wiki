---
type: entity
title: BMAD-METHOD
tags: [sdd, 多代理协作, ai工程]
related: [specflow, openspec, github-spec-kit, 多agent角色思维隔离, 规格驱动ai开发]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# BMAD-METHOD

BMAD-METHOD 是一种全能型**多代理协作工程框架**，模拟完整的专家团队（PM、架构师、QA），通过让 AI 在不同阶段扮演特定专家角色来提升产出质量。

## 核心特点

- **角色思维隔离**：不同阶段让 AI 扮演特定专家角色（PM、架构师、QA）
- **全能型**：覆盖从需求分析到质量保障的完整开发生命周期
- **专家团模拟**：通过 Prompt 为每个阶段分配专家角色

## 对 Specflow 的启发

- **角色思维隔离**理念被 Specflow 吸收，发展为 [[多agent角色思维隔离]] 机制
- Specflow 在此基础上定义了 PM、TL（技术负责人）、Dev（工程师）、Admin（知识管理员）四角色体系
- Specflow 2.0 进一步规划将角色 Prompt 模拟升级为独立的 Subagents