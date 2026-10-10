---
type: concept
title: autocompact水位线机制
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, 上下文压缩, token管理]
related: [三层上下文压缩, 九段式结构化摘要模板, microcompact工具白名单, 错误自愈三机制, 双压缩范式对比]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# autocompact水位线机制

**autocompact 水位线机制**是 [[claude-code]] 的自动压缩触发与分级回退策略：以 `AUTOCOMPACT_BUFFER_TOKENS = 13,000` 作为安全缓冲水位线，接近上限时自动触发压缩，并按「快路径优先、兜底保任务」回退。

## Session Memory Compact（Layer 2）保守策略

| 项目 | 值 |
|------|-----|
| 触发门槛 | Token ≥ 10,000 且文本消息 ≥ 5 条 |
| 单次压缩上限 | 40,000 token |
| 保留策略 | 严格保留最近几轮消息（保护"近因效应"） |
| 成本 | 复用已有会话记忆，零额外推理成本 |

## 回退链（Fallback）

SM Compact 快路径优先 → 不满足条件则回退 Full LLM Compact（[[九段式结构化摘要模板]]，精度与成本均最高）兜底——「正确时机用合适成本做恰到好处的压缩」。运行时错误 `prompt-too-long` 亦按同一三级顺序递进处理（见 [[错误自愈三机制]]）。整体三层框架见 [[三层上下文压缩]]，跨框架对比见 [[双压缩范式对比]] 与 [[上下文压缩策略]]。
