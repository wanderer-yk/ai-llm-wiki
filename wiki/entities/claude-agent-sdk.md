---
type: entity
title: Claude Agent SDK
created: 2026-09-30
updated: 2026-09-30
tags: [anthropic, sdk, agent, python, cli]
related: [claude-sonnet-4-5, ideatalk, aonesandbox, trade-spec, claude-code, anthropic, 脚手架优于模型]
sources: ["[202603301537]从VibeCoding到范式编程用Spec打造淘系交易的AI领域专家.html"]
---

# Claude Agent SDK

Claude Agent SDK 是 Anthropic 官方提供的 Python SDK，提供 `ClaudeAgentOptions` 与 `query()` 等 API，通过封装的 CLI 接口驱动 Agent 执行任务。需要说明：这是独立可验证的公开背景信息；在本源文章正文中，该 SDK 与 claude 系列产品被系统性匿名化为「SOTA模型」「SOTA模型_agent_sdk」，但在示例代码与运行结果中保留了原始标识（`ClaudeAgentOptions`、`ANTHROPIC_*` 环境变量、`claude-sonnet-4-5-20250929`）。

## 在本源中的用法

[[交易业务技术团队]]的 SOTA 通道以 Python 自建 Agent 框架，通过 Claude Agent SDK「用 Python 控制 CLI 执行任务，相当于跳过了 Agent 框架搭建这一步，直接复用 Anthropic 的工程成果」——自建三要素（Prompt 工程、工具编排、多轮对话状态管理）由成熟 SDK 一次性承接（与 [[脚手架优于模型]] 同构，与 [[三阶段平台演进路线]] 的「先验证后自建」决策闭环一致）。SOTA 模型被描述为「经过大量工程优化的成熟 Agent，具备代码检索、文件操作、多步推理、自我纠错等能力」。

## Demo 配置要点（逐字保留于 source 页）

- `env` 驱动配置：`DISABLE_PROMPT_CACHING="0"`（开启 prompt caching）、`ANTHROPIC_BASE_URL` 重定向至内部网关（见 [[ideatalk]]）、`ANTHROPIC_AUTH_TOKEN` 鉴权、`ANTHROPIC_MODEL="claude_sonnet4_5"`、`ANTHROPIC_SMALL_FAST_MODEL="claude-haiku-4_5"`（主副模型双配置）。
- `permission_mode='bypassPermissions'`：与全文「能力边界-安全底线/C3 合规」论述存在张力，沙箱弹内测试网语境可部分解释，生产治理方式未交代。
- 运行结果：2 turns、Bash `date` 工具调用成功、总成本 $0.079、耗时 9.7 秒、cache_read 15097 tokens（与 [[Token成本追踪]] 关联）。

## 关联

与 [[claude-code]]（SDK 封装的 CLI 接口与 Claude Code 架构一致）、[[anthropic]]（MCP 2024-11、Claude Sonnet 4.5 2025-09、Agent Skills 2025-10 三里程碑）、[[claude-sonnet-4-5]]（通道实际模型）直接相关。