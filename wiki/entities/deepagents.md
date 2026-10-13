---
type: entity
title: DeepAgents
tags: [agent框架, langchain, ralph-loop]
related: [langchain, ralph-loop, ralph成本控制]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# DeepAgents

DeepAgents 是 LangChain（langchain-ai）旗下的 Agent 框架（此归属依据来源文章及其引用的 GitHub 仓库 `langchain-ai/deepagents`），以原生支持 Ralph Loop 持续迭代模式为本文记录的核心特征（[[sources/[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式]]）。关联既有实体：[[langchain]]。

## Ralph 模式支持（来源证据）

- 官方示例路径：`langchain-ai/deepagents/tree/master/examples/ralph_mode`
- CLI 签名：`uv run deepagents --ralph "<任务>" --ralph-iterations 5`
- `--ralph-iterations` 参数承担迭代上限安全阀职能，与"实践建议-安全机制和资源控制"章节的 `--max-iterations` 铁律同构。

## 在 Ralph Loop 工具版图中的位置

文章"主流工具集成"清单成员之一，与 [[claude-code]] 官方插件 ralph-wiggum、[[kimi-cli]] 的 `loop_control` 配置、[[ralph-loop-agent]] 的编程式 JS 包并列，是其中唯一以框架级原生支持（而非插件或社区包）形式提供 Ralph 模式的工具。