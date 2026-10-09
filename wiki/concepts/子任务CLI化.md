---
type: concept
title: 子任务CLI化
tags: [并发调度, agent架构, harness-engineering]
related: [prompt确定性, 随到随补调度, 人工并发天花板, agent-control-plane]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# 子任务CLI化

百度Geek说《Harness Engineering》6.2 节核心决策：子任务**不在 Agent 对话内嵌套**，而是作为**独立 CLI 进程 + 独立 Agent 会话**，由外部脚本启动和管理。

## 四大好处

1. **Prompt 确定性**：`build-prompt.js` 程序化组装 Prompt，消除主 Agent"自由发挥"（→ [[prompt确定性]]、[[主agent转述失真]]）；
2. **Token 大幅降低**：消除上下文累积，且省去主 Agent 构建 Prompt 的消耗；
3. **并发数可控**：模型在对话内调度并发时**"过于谨慎"**，不愿开启几十/上百路并发；脚本则可任意设定并发数；
4. **确定性逻辑前置/后置**：脚本可在子任务前后插入预处理/后处理，完全不需 Agent 参与。

## 分工声明

"**Agent 只负责'审查代码并给出意见'这一步，其余全部由脚本完成**"——确定性逻辑（调度、状态、重试、合并）收归脚本，模型只做模型擅长的事。

## 关联

- 人侧 [[人工并发天花板]]（4-6 个并发）+ 模型侧并发过谨慎 → 并发控制交给脚本是两端的共同解；
- 脚本接管确定性控制呼应 [[agent-control-plane]] 方向；调度细节见 [[随到随补调度]]。