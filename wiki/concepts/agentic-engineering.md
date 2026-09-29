---
type: concept
title: Agentic Engineering（智能体工程）
tags: [方法论, ai工程化, agent, 结构化流程]
related: [vibe-coding, prompt-and-pray, skill-command-mcp三层架构, harness-engineering, 十一阶段后台开发流程, 研发范式前移, agent-control-plane]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# Agentic Engineering（智能体工程）

以 Agent 为核心的工程化开发方法论，由腾讯开发者 [[seanguo]] 于 2026 年 4 月系统阐述。核心主张：Vibe Coding（[[prompt-and-pray|"提示即祈祷"]]）仅适用于原型验证，生产环境必须升级为 Agentic Engineering——人在关键节点审核、AI 在结构化流程中自主执行。

## 核心分工原则

**人（Orchestrator/编排者）负责**：定义目标、拆解任务、审核方案、把关质量、最终决策

**AI（自主执行者）负责**：在结构化流程中自主执行重复性高、规则明确的工作

## 核心理念

> "Vibe Coding 依赖运气，Agentic Engineering 依赖流程。"

每个关键节点都有人工审核，AI 能力被约束在可控的工程框架里而非自由发挥。

## 工具载体：[[skill-command-mcp三层架构]]

- **Skill 层**：核心业务逻辑，含独立工具权限白名单和执行流程
- **Command 层**：轻量级路由入口（薄壳），自然语言可等价触发
- **MCP Server 层**：通过 MCP 协议连接的外部平台 API，配置一次全局生效
- **Superpowers 纪律链**：brainstorming→writing-plans→executing-plans 构成"理解→计划→执行"强制流程

## 与其他方法论的关联

### 跨公司方法论趋同

- 与爱奇艺 [[harness-engineering]] 的工程化约束思路高度一致：都是通过结构化流程防止 AI 自由发挥
- 与美团 [[pre-pr机制]] 概念对应：都是 AI 辅助审查 + 人工把关
- 与 [[agent-control-plane]] 的权限/边界/审计思路一致

### 与腾讯内部方法论的关系

- 与 binxiong 的 [[三大武器库]]（知识库+MCP+Skills）在 Skill/MCP 层面一致，但增加了 Command 层和 Superpowers 纪律子类
- 与 [[研发范式前移]] 的核心理念完全一致：先理解再动手→先计划再执行→有检查清单
- 与 [[增强自我而非取代自我]] 的定位理念呼应

### 关键分歧

- **文档策略**：[[三大武器库]] 对应 Skill/MCP 层，seanguo 新增 Command 薄壳层
- **流程粒度**：[[opsx指令集]]（8 条命令）vs [[十一阶段后台开发流程]]（11 阶段），架构重心不同
- **Command 定位**：binxiong 的指令可能包含更多内嵌逻辑，seanguo 的 Command 是薄壳委托

## 实证数据

基于 [[seanguo]] 一周实际实践的 [[十一阶段后台开发流程]]：11 个阶段中仅 4 个需要人工主动干预，总人工主动时间约 15-20 分钟。真实案例 RedeemReward 接口：4 Task 并行执行（49s~6m25s），自动生成 3 个 Conventional Commits。

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]