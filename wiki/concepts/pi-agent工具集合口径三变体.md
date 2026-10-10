---
type: concept
title: pi-agent 工具集合口径三变体
tags: [pi-agent, openclaw, 口径矛盾, 工具集]
related: [pi-agent, openclaw, 极简agent设计哲学, openclaw五层架构]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604280830]你不知道的Agent原理架构与工程实践.html", "[202604082000]从OpenClaw看Agent架构设计.html", "[202604131736]详尽地带你从零开始设计实现一个AIAgent框架.html"]
---
# pi-agent 工具集合口径三变体

同一系统（OpenClaw/pi-agent）的工具集合在三处来源中口径不一（review-9af27511 contradiction 立案）：

| 口径 | 来源 | 工具集 |
|---|---|---|
| ① 4 核心工具 | vivo《从 OpenClaw 看 Agent 架构设计》（[[wang-wenqian]]） | Read / Write / Edit / Shell |
| ② 五层架构表 | 千问AI平台《你不知道的 Agent》（[[侑夕]]）五层解耦表 | shell / fs / web / browser / MCP |
| ③ AgentLoop 代码 | 同上（千问篇）`registerDefaultTools` 注释 | shell / fs / web / message / cron |

## 初步裁定（待一手仓库终核）

三口径**不必然矛盾，疑为粒度与层位差异**：

1. ①是**Pi Agent 核心工具**的极简口径（对齐 [[极简agent设计哲学]] 的"4 工具足矣"叙事）；
2. ②是**产品整体工具面**（含浏览器与 MCP 接入——Gateway/Channel 层能力，非 Pi Agent core）；
3. ③是**AgentLoop 默认注册**的实现口径（message/cron 是消息与定时通道工具，属调度面）。

即：①=编辑器视角的核心四件、②=能力面视角、③=运行时注册视角。终核需比对 OpenClaw 一手仓库（openclaw 页登记待办）；本页仅记录三口径及来源，不裁决唯一正确口径。
