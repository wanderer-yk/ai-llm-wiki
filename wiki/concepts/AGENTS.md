---
type: concept
title: AGENTS.md
tags: [社区约定, 项目上下文, agent-skill]
related: [agent能力扩展演进, agent-skill, cursor, claude-code, ai友好研发规范]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# AGENTS.md

社区自发形成的约定：在仓库根目录放置一个自然语言的项目上下文与规范文件（`AGENTS.md`），让编码 Agent 了解项目背景。它源于 [[cursor]]、[[claude-code]] 等 Agent"能写代码但不了解项目"的痛点，处于 [[agent能力扩展演进]] 三阶段（MCP → AGENTS.md → Skill）的中间位置。

[[anthropic]] 推出 [[agent-skill]] 后，AGENTS.md 的理念被系统化为结构化知识包（指令 + 脚本 + 参考文档 + 资源文件）——Skill 可视为 AGENTS.md 的标准化、多级化演进形态。

与本 Wiki 既有概念的关联：其"以文件承载 AI 可执行的项目规范"思路与 [[ai友好研发规范]]（美团，规范从团队协作建议升级为约束 AI 产出的基础设施）在动机层一致。
