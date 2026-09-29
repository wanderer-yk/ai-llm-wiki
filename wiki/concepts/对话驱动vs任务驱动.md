---
type: concept
title: 对话驱动 vs 任务驱动
tags: [agent, 主循环设计, 范式]
related: [agent架构四决策, task-driven对goal-driven, agent-loop, 24h打工人]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604082000]从OpenClaw看Agent架构设计.html"]
---
# 对话驱动 vs 任务驱动

Agent 主循环设计的两种根本性范式：围绕对话框设计 vs 围绕感知-思考-行动循环设计。

## 对话驱动（Conversation-Driven）

对话框即 Agent 的世界——既是输入源、输出目标，也是维系上下文的容器。

- **优势**：交互门槛极低、模型天然兼容（RLHF 优化回复满意度）
- **问题**：一切行为绑定在对话交互中，无法支持多感知源（Cron/Webhook/fork 子任务）

## 任务驱动（Task-Driven）

感知→思考→行动循环。聊天框被降格为众多工具之一（与 execute、MCP 无本质区别）。

- **优势**：灵活、可观测性强、支持多感知源和多行动渠道
- **问题**：模型兼容性差（当前模型以对话模式训练）

## 思考过程作为核心输出

任务驱动中 Agent 核心输出变为可追溯的思考链（分析任务→制定方案→选择工具），而非对话回复。Claude Code 在此方向做了有意义的尝试。

可追溯性是持续改进的基础——可定位到具体哪一步思考出了问题。

## 模型对话训练倾向

任务驱动面临的现实约束：

- RLHF 优化"回复满意度"，直接给出答案几乎总比"我先调用工具"评分更高
- 工具调用是后天习得的能力，与回复产生竞争关系
- 从 GPT-4 到 Claude 到 Gemini 每一代工具调用能力在提升，但距离"模型天然就是任务执行器"还有距离
- 仅靠 system prompt 引导不够，需模型厂商在训练阶段针对 agent 模式专门优化

## 混合策略：对话前端 + 任务后端

用户通过对话提交需求 → 系统自动转化为任务 → 后台以任务驱动执行。

关键设计原则：**架构层面保留任务隔离能力**，而非将所有东西绑定在一个对话框上。

## 与 Wiki 已有概念的关系

- [[task-driven对goal-driven]]（zhiyuanfu/腾讯）— 本文的"对话驱动 vs 任务驱动"提供了该概念的技术实现维度。zhiyuanfu 关注 Task-Driven→Goal-Driven 的认知跃迁，本文关注 Task-Driven 的具体架构设计
- [[agent-loop]]（yabohe/腾讯）— While 循环是对话驱动模式的具体实现
- [[24h打工人]]（zhiyuanfu）— 本质上是任务驱动架构（Cron/Webhook 感知源 + execute/MCP 行动渠道）
- [[task-driven对goal-driven]] 进一步区分了 Task-Driven（解决执行问题）和 Goal-Driven（解决迭代问题），本文的任务驱动对应 Task-Driven 层面