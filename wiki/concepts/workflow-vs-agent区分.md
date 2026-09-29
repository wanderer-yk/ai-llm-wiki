---
type: concept
title: Workflow vs Agent 区分
created: 2026-06-22
updated: 2026-06-22
tags: [Workflow, Agent, 确定性, 理论基础]
related: [workflow优先于agent, anthropic, sop即智能, agent-skills]
sources: ["[202602271818]AgentSkills与MCP一场被误解的替代战争.html"]
---
# Workflow vs Agent 区分

**Workflow vs Agent 区分**是 [[anthropic]] 在《Building Effective Agents》中提出的理念，明确区分了两种 AI 任务执行范式。

## 定义区分

- **Workflow（工作流）** — 预定义路径编排多步 LLM 调用，确定性高，结果可控
- **Agent（智能体）** — LLM 自主决策工具调用和路径，灵活性强，但结果不确定性高

## Anthropic 的建议

在生产环境中**优先采用确定性较高的 Workflow**（即 [[agent-skills]] 编排的业务流程），而非完全自主的纯 Agent。只有在需要高度灵活性的场景才使用 Agent。

## 与 Wiki 既有概念的关系

- [[workflow优先于agent]] 是本理念在有赞 AI 客服场景中的直接实践
- [[sop即智能]] 是本理念的理论基础（结构化流程 > 自主推理）
- 与 [[ai-native研发模式]] 中"AI 从打字员到施工队长"的渐进路径一致：先确定性后自主性
- [[三大武器库]] 的知识库+MCP+Skills 架构是 Workflow 与 Agent 结合的实践形式