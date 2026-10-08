---
type: concept
title: Agent 能力扩展演进
tags: [mcp, AGENTS, agent-skill, 演进史]
related: [agent-skill, mcp, AGENTS, anthropic, claude-code, cursor, 三大武器库, skill-command-mcp三层架构]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Agent 能力扩展演进

Agent "学新技能"方式的三阶段演进叙事与收敛判断：MCP（工具连接）→ AGENTS.md（项目上下文）→ Skill（系统化知识包）。本文给出的定论是收敛论：**"Skill 正在统一 Agent 能力扩展的途径"**。

## 三阶段路径

1. **MCP（2024-11，Anthropic 开源）**：解决 Agent"连接"问题的基础设施层突破，让 Agent 能读 GitHub、查 Sentry、操作 Linear——但不解决"按什么流程、顺序、标准使用工具"。
2. **AGENTS.md（社区约定）**：社区自发在仓库根目录放置自然语言项目上下文与规范文件，源于 [[cursor]]、[[claude-code]] 等 Agent"能写代码但不了解项目"的痛点（详见 [[AGENTS]]）。
3. **Skill（2025-10，Anthropic 推出）**：把 AGENTS.md 理念系统化为结构化知识包（指令 + 脚本 + 参考文档 + 资源文件），随后 Cursor、Windsurf 等跟进类似机制（见 [[agent-skill]]、[[skills-cli跨平台安装]]）。

## 收敛论 vs 分层论（待裁决张力）

- 本文（单来源判断）：演进正在收敛，Skill 为终局形态；论据还包括渐进式披露提升注意力分配效率、自然语言知识表达比硬编码逻辑更灵活更 Agentic。
- [[三大武器库]]（腾讯 binxiong）与 [[skill-command-mcp三层架构]]（seanguo）：知识库 + MCP + Skills 三层并存，Skill 仅为其中一层。

两种框架可能是视角差异（能力扩展机制史 vs 企业工具栈分层），也可能是实质分歧，需综合页裁决。
