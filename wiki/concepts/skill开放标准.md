---
type: concept
title: Skill 开放标准
created: 2026-10-13
updated: 2026-10-13
tags: [skill, anthropic, 开放标准, 规范]
related: [anthropic, skillmd格式规范, skill渐进式加载机制, 模型驱动触发逻辑, skill生态知识体系]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 开放标准

Skill 开放标准是 Anthropic 于 2025 年 12 月发布的 Agent Skill 开放规范：定义 Skill 的文件格式、目录结构与加载行为的公共协议。官方规范地址为 https://agentskills.io/specification ，官方仓库为 https://github.com/anthropics/skills 。

## 采纳情况

据来源声明，已被 **33+ 个 Agent 产品**采纳，包括 Claude Code、OpenAI Codex、GitHub Copilot、VS Code、Cursor、Gemini CLI、Kiro 等。该量化声明已登记 [[ai工程量化效果声明追踪]]，外部核实待完成。

## 标准边界

规范只定义"Skill 长什么样"（格式与外形），不定义"内部逻辑该怎么设计"——内部逻辑设计属 [[skill五种设计模式]]（Google）与构建方法论（[[skill-creator]] / Writing-Skills）的范畴。

## 生态位

openclaw/clawhub 等 6 个开源 Skills 市场（skills.sh、skillsmp.com、clawhub、qoder-community、alicloud-skills、hermes-agent）与 1060+ Skills 的 Awesome 列表（VoltAgent/awesome-agent-skills）以及 skillsbench.ai、2 篇 arxiv 评测论文共同佐证该标准的生态发展速度。clawhub 的列入为 [[openclaw]] 补充生态位证据。
