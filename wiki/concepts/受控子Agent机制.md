---
type: concept
title: 受控子Agent机制
tags: [多agent系统, 安全隔离, hermes-agent]
related: [hermes-agent, 原子agent设计三原则, agent权限边界清单, mailbox消息通道, 多agent交叉审核, 人工并发天花板, 全生命周期hook机制, 多层安全护栏]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# 受控子Agent机制

受控子Agent机制是一种多 Agent 委派设计：允许把复杂任务委托给 Sub-Agent 并行处理，同时对子 Agent 实施严格的沙箱隔离——受限工具集、单向任务分解关系、片段化上下文授权。其目标是既获得并行分解的效率，又防止权限升级与失控嵌套。来源文章将其归于 [[hermes-agent]] 的 `tools/delegate_tool.py` 实现。

## 三条隔离原则

1. **禁止递归委派**：防"套娃"，任务分解保持受控深度
2. **禁止反向询问**：子 Agent 不能向父 Agent 或用户追问，保证任务分解单向线性
3. **上下文/记忆片段化授权**：子 Agent 只获得任务必要片段，兼顾数据安全与真·并行互不干扰

## 代码级约束（`tools/delegate_tool.py`）

```makefile
# 子Agent不能使用的工具（防止权限升级）
DELEGATE_BLOCKED_TOOLS = {
    "delegate_task",     # 防止递归委派（子Agent不能再创建子子Agent）
    "clarify",           # 防止嵌套提问循环
    "memory",            # 防止操纵记忆
    "send_message",      # 防止消息劫持
    "execute_code"      # 防止代码执行权限升级
}

MAX_CONCURRENT_CHILDREN = 3    # 最多 3 个并行子Agent
MAX_DEPTH = 2                  # 最多 2 层嵌套
```

五项黑名单分别对应五类风险：防递归委派、防嵌套提问循环、防操纵记忆、防消息劫持、防代码执行权限升级。子 Agent 完成任务后由 `on_delegation()` Hook 回调（见 [[全生命周期hook机制]]）。

## 内部口径张力（需注意）

来源散文表述"子 Agent 不能再次创建新的子 Agent"（即仅一层委派），但代码注释 `MAX_DEPTH = 2 # 最多 2 层嵌套` 暗示允许两层——两处口径不一致，引用时需注明。

## 与相关概念的关联

- 与 [[原子agent设计三原则]] 的"复合禁止嵌套"原则高度同构：都以禁止无限嵌套控制复杂度
- 黑名单式权限边界与 [[agent权限边界清单]] 思路一致：显式列举禁止项而非穷举允许项
- "禁止反向询问"与 [[mailbox消息通道]] 的双向通信形成对照：Hermes 选择单向线性分解换取确定性
- `MAX_CONCURRENT_CHILDREN = 3` 与 [[人工并发天花板]]（人脑管理多 AI 终端约 4-6 个并发）呼应：机器侧并发上限同样需要显式设定
- 沙箱隔离是 [[多层安全护栏]] 的组成部分；委派与 [[多agent交叉审核]] 分属"任务分解"与"质量把关"两种多 Agent 用法
