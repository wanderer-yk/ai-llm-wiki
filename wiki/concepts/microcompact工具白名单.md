---
type: concept
title: microcompact工具白名单
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, 上下文压缩, kv-cache, token优化]
related: [三层上下文压缩, autocompact水位线机制, 快照冻结与前缀缓存, 双压缩范式对比, 函数结果清理机制]
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# microcompact工具白名单

**microcompact 工具白名单**是 [[claude-code]] 三层压缩体系第一层 MicroCompact（`src/services/compact/microCompact.ts`）的核心配置：`COMPACTABLE_TOOLS` 仅压缩 Bash、Read、Grep、Glob 等大量标准输出工具的结果，而 Edit/Write 等核心状态变更输出完整保留——「抓大放小」。作者评价其为结构化工具输出压缩中 ROI 最高的选择，反驳了「压缩必须靠大模型总结」的认知。

## 两条压缩路径

| 路径 | 机制 |
|------|------|
| 基于时间 | 截断超时间阈值旧消息的工具输出（具体阈值未披露） |
| 基于缓存 | 智能识别 KV Cache 边界，仅在边界外压缩，最大化缓存命中率（与 [[cacheScope分级缓存]]、[[快照冻结与前缀缓存]] 直接关联） |

另附工程近似：多模态图片统一按 2000 token 估值，以微小误差换取显著性能提升。

MicroCompact 无 LLM 参与、规则驱动、极致轻量，是三层递进（→ [[autocompact水位线机制]] 所辖 SM Compact → Full LLM Compact/[[九段式结构化摘要模板]]）中成本最低的一层；整体框架见 [[三层上下文压缩]] 与 [[双压缩范式对比]]。
