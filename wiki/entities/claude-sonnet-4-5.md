---
type: entity
title: claude-sonnet-4-5
created: 2026-09-30
updated: 2026-09-30
tags: [anthropic, 模型, llm]
related: [claude-agent-sdk, anthropic, trade-spec, ideatalk]
sources: ["[202603301537]从VibeCoding到范式编程用Spec打造淘系交易的AI领域专家.html"]
---

# claude-sonnet-4-5

claude-sonnet-4-5 是 Anthropic 于 2025 年 9 月发布的大语言模型（文中能力声明：在约 20 万 token 的上下文窗口下，可长时间、稳定地「挂住」整个代码仓库、技术方案和运行日志，支持连续数十小时的自动编码与调试；该发布时间与公开史实相符）。在本源中，它是 [[trade-spec]] 双通道架构中 SOTA 通道的实际模型：文章正文将其匿名化为「SOTA模型」，而 Demo 代码配置 `claude_sonnet4_5` 与返回消息体中的 `model='claude-sonnet-4-5-20250929'` 直接点名了具体型号，解决了贯穿全文的「SOTA 模型=Claude Sonnet 4.5 口径」悬案。

## 本源中的证据与数据

- 能力定位：行业智能体自治能力的天花板（Agentic Autonomy），极高自主规划与多步推理、最繁荣的 MCP 生态与自定义指令体系；业务实施标准可落地为「SOTA模型.md 规则」快速验证。
- MVP Demo：经 [[ideatalk]] 内部网关调用，2 turns、Bash 工具调用、总成本 $0.079、耗时 9.7 秒。
- 配套小模型：`claude-haiku-4_5`（`ANTHROPIC_SMALL_FAST_MODEL` 配置，主副模型双配置中的小快模型）。

## 关联

[[claude-agent-sdk]]（调用载体）、[[anthropic]]（发布方，另发布 [[mcp]] 与 Agent Skills）、[[trade-spec]]（使用主体）。