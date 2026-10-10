---
type: concept
title: Text > Brain 落盘原则
tags: [openclaw, 记忆管理, prompt-engineering]
related: [openclaw, openclaw工作区md文件族, markdown多层记忆体系, 记忆写入双路径, memory-flush机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Text > Brain 落盘原则

Text > Brain 落盘原则是 [[openclaw]] AGENTS.md Memory 分层规则中的核心指令："Mental notes don't survive session restarts. Files do."——脑内记事无法在会话重启后存活，文件才能。因此记事、教训、错误必须写文件，不依赖"心里记着"。

该原则与 SOUL.md 的 Continuity 段字面互证："These files *are* your memory. Read them. Update them. They're how you persist."。工程实现上，它与写入双路径（显式写入 + Memory Flush 隐式闪存，见 [[记忆写入双路径]] 与 [[memory-flush机制]]）和 [[markdown多层记忆体系]] 共同构成 OpenClaw 记忆落盘体系：一切长期价值信息必须经"落盘"进入 [[openclaw工作区md文件族]]，才能跨会话存活。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
