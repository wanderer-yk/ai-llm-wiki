---
type: concept
title: Compaction 双触发模式
tags: [openclaw, context-engineering, 上下文压缩]
related: [openclaw, 自适应分块压缩, 摘要分层降级策略, context-window三段构成, autocompact水位线机制, 上下文压缩策略]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Compaction 双触发模式

Compaction 双触发模式是 [[openclaw]] 上下文压缩的两种触发方式：①**手动触发**——`/compact` 命令，可携带指令指定保留内容；②**自动触发**——水位线机制，当 `当前 token 用量 > 上下文窗口大小 - 预留空间` 时触发。示例：20 万窗口预留 2 万，超过 18 万即触发压缩；触发后保留最近 5 轮对话原文、压缩其余 N-5 轮为摘要。

压缩的算法细节（自适应分块）见 [[自适应分块压缩]]，摘要生成与降级见 [[摘要分层降级策略]]。该机制与 Claude Code 的 [[autocompact水位线机制]] 构成跨框架对照（同为水位线思路），并为 [[上下文压缩策略]] 中"OpenClaw 分阶段压缩"的描述提供源码级证实。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
