---
type: source
title: 从 OpenClaw 看 Agent 架构设计
authors: [vivo 互联网搜索团队]
year: 2026
url: ""
venue: 微信公众号「vivo 互联网技术」
tags: [agent, 架构设计, openclaw, 上下文管理, 工具加载, skill, 主循环]
related: [openclaw, claude-code, agent-architecture-design, append-only-context, compression-strategy, task-isolation, prompt-cache-mechanism, skill-as-knowledge-cache, conversation-driven-vs-task-driven]
created: 2026-06-08
updated: 2026-06-08
sources: ["从OpenClaw看Agent架构设计.html"]
---

# 从 OpenClaw 看 Agent 架构设计

## 基本信息

- **标题**：从 OpenClaw 看 Agent 架构设计
- **作者**：vivo 互联网搜索团队（含王文乾等）
- **发布渠道**：微信公众号「vivo 互联网技术」
- **发布时间**：2026 年 04 月 08 日
- **文章属性**：原创
- **核心对象**：[[openclaw]]，并与 [[claude-code]]、ChatGPT、Gemini 等主流 Agent 进行横向对比

## 摘要

本文从工程视角系统剖析了构建大模型 Agent 时必须面对的**四大关键架构决策**，并指出这些决策没有"标准答案"，只有"明确的代价与妥协"：

1. **上下文管理**：[[append-only-context|追加式上下文]]、[[compression-strategy|压缩策略]]、[[task-isolation|任务隔离]]
2. **工具加载**：[[prompt-cache-mechanism|Prompt 缓存]] 与动态加载的冲突、[[console-vs-mcp-strategy|控制台+MCP 混合]]、[[progressive-tool-loading|渐进式加载]]
3. **工具查找**：[[skill-based-organization|技能维度组织]] 与 [[skill-as-knowledge-cache|Skill 作为知识缓存]]
4. **主循环设计**：[[conversation-driven-vs-task-driven|对话驱动 vs 任务驱动]] 与 [[perceive-think-act-loop|感知-思考-行动循环]]

文章最终提出：[[architectural-decision-interdependence|四大决策相互交织]]，最务实的架构解法是**对话作为前端收集需求，任务作为后端隔离执行**的混合模式。

## 核心主张

- 构建 Agent 没有标准答案，每个架构决策都有明确代价。
- 追加式上下文模式需结合压缩机制与任务隔离作为"安全网"。
- [[skill-as-knowledge-cache|Skill]] 是建立在搜索与上下文追加之上的**缓存层**，能极大降低 Token 消耗并形成经验沉淀，且可由 Agent 自动生成实现自我优化。
- 任务驱动模式将[[thought-process-as-first-class-citizen|思考过程]]作为一等公民输出，解决黑盒问题，使错误可追溯。
- 受限于 [[model-training-bias|RLHF 训练倾向]]，纯任务驱动落地受限，可通过"对话转任务"模式优雅融合。

## 主要矛盾与权衡

| 维度 | 矛盾 |
|------|------|
| 工具加载 | 动态工具加载（灵活性）vs Prompt 缓存稳定性（成本控制） |
| 工具查找 | 接口维度（模型原生）vs 功能维度（实际需求） |
| 主循环 | 任务驱动架构愿景 vs RLHF 导致模型偏好"直接对话回复" |

## 关键证据

- 上下文交互 Token 增长图（2K → 80K）及 Opus 定价下切换话题成本 $0.30 示例
- Claude Code 报告的 92% 缓存命中率与其永不改变 `tools` 列表的关系
- Claude Code 与 OpenClaw 压缩策略对比表
- 有/无 Skill 的执行步骤与 Token 消耗对比
- 对话驱动 vs 任务驱动架构 ASCII 图与输出对比图

## 开放问题

- 如何设计跨任务的高效信息传递机制？
- 需要持久连接或复杂状态管理（如 WebSocket、连接池）的工具如何高效加载？
- 模型厂商应如何针对 Agent 模式优化训练，弱化直接回复本能？

## 相关概念

- [[agent-architecture-design]]
- [[prompt-based-tool-injection]]
- [[append-only-context]]
- [[compression-strategy]]
- [[task-isolation]]
- [[perceive-think-act-loop]]
- [[thought-process-as-first-class-citizen]]