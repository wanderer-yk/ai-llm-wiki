---
type: entity
title: Skill-Creator
tags: [anthropic, 工具, agent-skill, 评测]
related: [anthropic, skill-creator三版演进, agent-skill, skill评测三原则]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill-Creator

Anthropic 官方提供的 Skill 创建工具，收录于 `anthropics/skills` 官方仓库（同名 Skill 为 `skill-creator`）。它不是静态工具，而是经历了三个版本的演进（详见 [[skill-creator三版演进]]）：第一版"创建"（自然语言描述 → SKILL.md，降低上手门槛）→ 第二版"创建 + 优化"（承接几乎所有 Skill 相关工作、description 更激进、可自主改进 Skill）→ 第三版"自动评测优化"（生成评测用例、创建评分机制、运行评测、评价汇总、循环改进，对最终运行效果负责）。

在《打造高效易用的 Agent Skill》中，Skill-Creator 的演进被用作案例，论证 Skill 工具链的成熟路径：从"生成"到"生成+优化"再到"对最终运行效果负责"——即评测内建于工具本身（呼应 [[skill评测三原则]]）。

## 开放问题

- 三版演进的时间线与发布节奏未在来源中交代。
