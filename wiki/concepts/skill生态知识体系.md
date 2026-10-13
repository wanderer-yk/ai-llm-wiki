---
type: concept
title: Skill 生态知识体系
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 生态, 知识体系, anthropic, google-adk]
related: [skill开放标准, skill-creator, writing-skills, skill五种设计模式, skill不是prompt]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 生态知识体系

Skill 生态知识体系是对 Agent Skill 生态知识的三层组织框架：**规范标准（agentskills.io）→ 构建方法论（Anthropic/Superpowers）→ 设计模式（Google）**。该框架由《Agent Skill规范、构建与设计模式》作者[[珂罗]]在总结章提出，用于将全文的双厂商内容组织为一条完整知识链。

## 三层链

| 层 | 主体 | 内容 |
|---|---|---|
| 规范标准 | Anthropic（agentskills.io/specification） | SKILL.md 格式、目录结构、渐进式加载、触发机制 |
| 构建方法论 | Anthropic Skill-Creator + Superpowers Writing-Skills | ML 式评测路线 + TDD 式路线 + 双 Claude 迭代式 |
| 设计模式 | Google ADK（Google Cloud Tech） | Tool Wrapper / Generator / Reviewer / Inversion / Pipeline |

## 意义

- 为 Agent 能力封装（Skills）这一层提供了从"是什么/怎么写"（规范）、"怎么造"（方法论）到"怎么设计内部逻辑"（模式）的完整知识栈
- 与本 wiki 既有概念簇互补：[[生产级skill]]（腾讯视角的可复用 SOP 封装）、[[skill-command-mcp三层架构]]（工具分层）、[[function-calling-mcp-skills本质差异对比]]（规范层对照）
- 开放问题："构建方法论"层内部三条路线（ML 式/TDD 式/双 Claude 人工观察式）的适用边界来源未做正面对比，需 wiki 侧综合裁决
