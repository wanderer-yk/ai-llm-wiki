---
type: concept
title: Agent Skill（知识包）
tags: [agent-skill, anthropic, 知识包, 能力扩展]
related: [agent能力扩展演进, skill渐进式披露, skill三阶段工作原理, skill目录结构与命名规范, description触发准确性权衡, skill-body两种形态, anthropic, 生产级skill, 三大武器库, 知识库降熵论, workflow优先于agent, 最大化复用]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Agent Skill（知识包）

Agent Skill 是把经验、流程和最佳实践封装为 Agent 可反复调用的知识包的标准化能力扩展方案，由 Anthropic 于 2025 年 10 月推出。它在文件系统中是一个文件夹：以 `SKILL.md` 为入口（YAML Frontmatter 的 `name`/`description` 必填），可选挂载 `scripts/`（可执行脚本）、`references/`（参考文档）、`assets/`（资源文件），详见 [[skill目录结构与命名规范]]。

## 动机：Agent 私域知识缺失

大模型很聪明，但没有用户的私域知识和专属能力——每次对话重新教既低效又不稳定；即使通过 [[mcp]] 获得工具调用能力（读 GitHub、查 Sentry、操作 Linear），Agent 依然不知道按什么流程、顺序、标准使用工具。Skill 补足这一层（见 [[agent能力扩展演进]]）。

## 核心比喻与复用优先

- **新人/工作手册比喻**：通用 Agent 像聪明但不了解你业务的新人，Skill 就是你给他的工作手册——Agent 拿到后像拿到工作手册一样自主执行。
- **复用优先**：Anthropic 官方（PDF/DOCX/PPTX、前端设计）与社区工作流 Skill 拿来即用，定制前先复用现有 Skill；这与天猫 [[最大化复用]] 的工程共性一致。

## 本质

把本来每次都要重新交代的经验、流程和标准，整理一次存下来，之后 Agent 自己就知道该怎么做——省掉重复劳动，换来稳定可预期的输出。"稳定可预期输出"与有赞 [[workflow优先于agent]] 的结果可控性优先形成跨来源共识；知识沉淀降低人机协作传递成本则与 [[知识库降熵论]] 同构。

## 与既有概念的关系

- [[生产级skill]]（腾讯）：从团队工程化视角定义"可复用 SOP 封装"，与本文的官方 Skill 格式方法论互补，合并决策待综合处理。
- [[三大武器库]]（腾讯）与 [[skill-command-mcp三层架构]]（seanguo）：将 Skill 视为分层体系中的一层；本文则主张 Skill 正在统一能力扩展途径（收敛论），存在待裁决的张力。

## 双要素编写观

Description 决定"什么时候用"（[[description三大要素]]、[[description触发准确性权衡]]），Body 决定"用起来效果如何"（[[skill-body两种形态]]）——这是 Skill 能用与好用差距的核心。
