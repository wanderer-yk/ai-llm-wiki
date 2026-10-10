---
type: concept
title: OpenClaw System Prompt 23 模块组装矩阵
tags: [openclaw, prompt-engineering, system-prompt, 源码拆解]
related: [openclaw, promptmode三级模式, openclaw工作区md文件族, system-prompt动态组装机制, 心跳机制heartbeat]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# OpenClaw System Prompt 23 模块组装矩阵

OpenClaw 的 System Prompt 并非固定文本，而是由 `src/agents/system-prompt.ts` 中的 `buildAgentSystemPrompt()` 函数在运行时动态拼装：接收几十个参数，按固定顺序组合约 23 个模块，每个模块有明确的加载条件（永远存在 / full / minimal / 有技能时 / 有记忆工具时 / 有网关工具时等）。本矩阵是 wiki 中 OpenClaw Prompt 层最完整的一手证据。

| # | 模块 | 加载条件 | 要点 |
|---|------|---------|------|
| 1 | 身份标识 | 永远存在 | `You are OpenClaw, a personal AI assistant.`（none 模式也保留） |
| 2 | 工具清单 | full/minimal | 工具名区分大小写，必须严格按名调用 |
| 3 | 工具调用风格 | full | 简单任务直接调用不解释；复杂任务先告知再动手 |
| 4 | 安全准则 | full | Safety Guidelines 六条 |
| 5 | CLI 操作指令 | full | `/status`、`/new`、`/compact`（手动触发上下文压缩）、`/think`、`/usage` |
| 6 | 技能（Agent Skills） | full，有技能时 | 先扫描技能列表（仅名字和描述），命中才读 SKILL.md |
| 7 | 记忆召回 | full，有记忆工具时（`memory_search`/`memory_get` 可用） | 防幻觉规则：涉既往必先 `memory_search`，低置信度须明说（"say you checked"） |
| 8 | 自更新管理 | full，有网关工具时 | gateway 管理指令 |
| 9 | 模型别名 | full/minimal，有配置时 | `claude-opus -> claude-opus-4-6`、`claude-sonnet -> claude-sonnet-4-6`、`gpt-4o -> gpt-4o-latest` |
| 10 | 工作区信息 | full/minimal | `Working directory: ~/.openclaw/workspace` |
| 11 | 参考文档 | full，有路径时 | docs.openclaw.ai / github.com/openclaw/openclaw / discord.com/invite/clawd / clawhub.com |
| 12 | 沙箱（Sandbox） | full，沙箱模式时 | Docker 容器、`/workspace` 挂载、提权需显式策略 |
| 13 | 授权发送者 | full，有配置时 | 真实身份哈希处理；allowlist 不默认为 owner |
| 14 | 时间信息 | full/minimal，有配置时 | 时区（示例 `Asia/Shanghai`） |
| 15 | Workspace 文件注入 | full/minimal | AGENTS.md / SOUL.md / USER.md / IDENTITY.md / TOOLS.md 全文注入（与 SKILL.md 渐进式披露不同，是直接注入） |
| 16 | 回复标签 | full | `[[reply_to_current]]` 原生引用回复 |
| 17 | 消息系统 | full | 当前会话自动路由回源 channel；跨会话用 `sessions_send(sessionKey, message)`；子 Agent 编排用 `subagents(action=list\|steer\|kill)` |
| 18 | 语音合成（Voice/TTS） | full，有 TTS 时 | 语音相关指示 |
| 19 | 群聊回复（Reactions） | full，有配置时 | 表情 vs 文字回复时机 |
| 20 | 推理格式（Reasoning） | 启用"深度思考"模式时 | 回复中展示推理过程的方式 |
| 21 | 静默回复 | full | 无需回复时精确输出 `[SILENT]` |
| 22 | 心跳机制（Heartbeats） | full | 定期唤醒执行定时任务；无事回复 `HEARTBEAT_OK` |
| 23 | 运行时信息（Runtime） | 永远存在 | `agentId / host / os / model / shell / channel / capabilities` |

模块 15 注入的五个文件详见 [[openclaw工作区md文件族]]；模块 22 的机制详见 [[心跳机制heartbeat]]。加载范围由 [[promptmode三级模式]] 决定。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
