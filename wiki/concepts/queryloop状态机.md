---
type: concept
title: QueryLoop 状态机
tags: [claude-code, query-loop, 运行时, 状态机]
related: [queryloop状态机, claude-code, 确定性逻辑外置, autocompact水位线机制, 三层上下文压缩, 函数结果清理机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html"]
---
# QueryLoop 状态机

QueryLoop 状态机是无岳《Claude Code 源码拆解》对 `query()` 内核的结构化定性：agent turn 不是线性管道而是一个**被反复改写的「运行」**——压缩、恢复、工具结果回灌、预算约束、用户中断都会中途改写其状态，因此状态必须升格为 runtime 对象而非函数局部变量。

## 双层结构

`query` 呈 [[query双层结构]]：headless 会话外壳（面向脚本/CI 的无界面调用）+ 状态机内核（面向交互会话的持续运行）。

## 四类 runtime 机制

1. **长上下文治理**：snip / microcompact / collapse / autocompact 多档压缩（与 [[autocompact水位线机制]]、[[三层上下文压缩]]、[[microcompact工具白名单]]、[[函数结果清理机制]] 跨源互证）；
2. **失败恢复**：reactive compact / max output recovery / fallback model；
3. **[[工具结果协议化回灌]]**：工具结果按协议格式回注上下文；
4. **[[空隙期异步预取]]**：等待模型响应的空隙执行预取。

## 归纳

- [[运行时课题论]]：运行时问题（而非模型能力）决定 Agent 存活；
- [[坏路径主路径设计]]：异常路径是设计对象而非补丁。
