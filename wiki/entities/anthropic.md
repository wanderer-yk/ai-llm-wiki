---
type: entity
title: Anthropic
created: 2026-06-22
updated: 2026-10-08
tags: ["ai-company", "llm-provider", "agent-research", "组织", "AI公司", "MCP", "Agent", "llm", "context-rot", "agent-skill", "claude"]
related: ["openai", "claude-3-与-claude-3-5", "f-harness", "harness衰变定律", "AI自恋问题", "mcp", "claude-code", "workflow-vs-agent区分", "sop即智能", "claude-opus", "上下文腐烂", "agent-skill", "claude-agent-sdk", "claude-sonnet-4-5", "Skill-Creator", "agent能力扩展演进"]
sources: ["[202602041445]从回答者进化为研究员全面解析DeepResearch.html", "[202603091800]打造高效易用的AgentSkill.html", "[202605201731]干货从PromptContext到Harness工程的三次进化与终局之战原创.html"]
---

# Anthropic

Anthropic 是一家专注于 AI 安全与大型语言模型的美国公司，Claude 系列大语言模型的开发者，同时也是 [[mcp]]（Model Context Protocol）和 [[claude-code]] 的提出方/开发方。以上为独立可验证的基本背景；本页其余陈述以本 Wiki 各来源文章的表述为准。

在本 Wiki 来源中，Anthropic 被置于两个叙事语境：

- 在 [[李伟山]] 的文章中，Anthropic 被作为两大核心实践案例之一加以引用；
- 在《打造高效易用的 Agent Skill》一文中，Anthropic 是 Agent 能力扩展生态的中心主体。

## 对 Agent 工程的贡献

### Building Effective Agents 指南

Anthropic 发布了《Building Effective Agents》指南，明确区分了 Workflow（确定性编排）与 Agent（自主决策），支持在生产环境中**优先采用确定性较高的工作流**而非完全自主的纯 Agent。这一理念是 [[workflow优先于agent]] 的理论基础之一，也为 [[agent-skills]] 作为业务流程编排层的价值提供了理论支撑。

### MCP

Anthropic 于 2024 年 11 月开源 [[mcp]]，定位为解决 Agent"连接"问题的基础设施层突破（见 [[agent能力扩展演进]]）。此后 MCP 已成为行业标准的工具/资源/外部系统接入协议，与 [[agent-skills]] 形成"能力提供"与"流程编排"的互补关系。

## 在 Agent Skill 生态中的角色

> 以下内容来自《打造高效易用的 Agent Skill》一文对 Anthropic 角色的梳理。

在该文语境中，Anthropic 承担五个角色：

1. **MCP 开源方**：2024 年 11 月开源 [[mcp]]（详见上文 MCP 一节）。
2. **Agent Skill 推出方**：2025 年 10 月推出 [[agent-skill]] 标准化方案，把 AGENTS.md 理念系统化为结构化知识包。
3. **官方 Skill 供给方**：维护 `anthropics/skills` 官方仓库，提供 `pdf`、`skill-creator`、`frontend-design` 等高质量 Skill，被称为"渐进式披露和脚本自动化的最佳实践"学习范本。
4. **脚本封装模式示范者**：官方 PDF/DOCX/PPTX 等文档生成 Skill 大量采用"文档生成逻辑封装在 Python 脚本、SKILL.md 只负责告知何时调用哪个脚本传什么参数"的模式（见 [[skill脚本自动化原则]]）。
5. **Skill-Creator 官方工具方**：提供 [[Skill-Creator]] 工具，其三版演进被该文作为 Skill 工具链成熟路径案例。

## F-Harness 三角色机制

> 以下内容来自 [[李伟山]] 文章对 Anthropic 研究的引用。

文章引用 Anthropic 的研究发现，提出 [[f-harness]]（三角色分工机制）：

- **Planner**：规划任务分解
- **Generator**：执行代码生成
- **Evaluator**：独立审查产出质量

F-Harness 的设计动机是解决 [[AI自恋问题]]——AI 倾向于给自己产出打高分的系统性偏差。Anthropic 的 Claude.ai 克隆界面实验证明，单 Agent 存在中途遗忘、虚报完成、自评过度乐观三大问题。

**待确认**：F-Harness 是否为 Anthropic 官方术语，还是本文作者的概括，有待考证。

## Harness 衰变定律

> 以下内容来自 [[李伟山]] 文章对 Anthropic 研究的引用。

文章引用 Anthropic 研究中发现的 [[harness衰变定律]]：模型能力越强，所需 Harness 越简单。Claude 3.0 → Claude 3.5 的升级使许多 Harness 规则自然失效。

## 上下文腐烂（Context Rot）

Anthropic 提出并命名了"[[上下文腐烂]]"（Context Rot）现象。该现象指出：随着上下文长度爆炸式增长，模型出现性能下降、注意力分散、信息利用效率降低（如"Lost in the middle"）的负面现象。

Anthropic 的大模型 [[claude-opus|Claude Opus]] 虽然支持长输出，但在 Agent 频繁调用工具时同样受限于上下文腐烂现象。

## 产品与模型

- **[[claude-code|Claude Code]]**：开发方为 Anthropic；《打造高效易用的 Agent Skill》一文给出了其 Skill 安装路径约定与 `view`/`read` 内置工具。
- **Claude.ai**：Anthropic 的 AI 助手产品，用于克隆复杂界面实验的场景对象。
- **[[claude-opus|Claude Opus]]**：支持长输出的大模型，受限于上下文腐烂。
- **Claude 3 / Claude 3.5**：详见 [[claude-3-与-claude-3-5]]。

另见：[[claude-agent-sdk]]、[[claude-sonnet-4-5]]。