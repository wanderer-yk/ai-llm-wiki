---
type: concept
title: Stop Hook 拦截机制
tags: [hooks, claude-code, 循环控制, ralph-loop]
related: [ralph-loop, 机器可验证完成条件, 循环控制外部化, llm自我评估不可靠, claude-code, agent-control-plane]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# Stop Hook 拦截机制

Stop Hook 拦截机制是 Ralph Loop 的技术使能机制：当模型尝试结束会话时，外部 hook 脚本（如 `hooks/stop-hook.sh`）拦截退出信号并以退出码 2 截断，扫描模型输出中是否含有完成承诺（Completion Promise）；未发现 `<promise>COMPLETE</promise>` 则重载原始提示词强制继续循环——文章概括为"未达客观成功标准就无法下班"。

## 工作流程

1. 模型输出退出意图
2. Stop Hook 以退出码 2 截断退出信号
3. 外部脚本扫描输出，查找完成承诺标记
4. 找到 → 允许退出；未找到 → 重载原始提示词，会话继续

## 定位

- 工业实现依赖对终端交互的深度拦截，是 [[claude-code]] hooks 能力的应用。
- 与 [[机器可验证完成条件]] 配合：Hook 是执行者，完成承诺是判定依据。
- 与 [[循环控制外部化]] 构成机制与哲学的关系：Hook 是"决策权外置"的具体实现层。

简版 bash 循环（无 hook，靠 grep 扫描输出）是同一机制的去依赖变体；`hooks/stop-hook.sh` 是终端集成变体。

## 跨来源关联

从"控制平面"视角看，Stop Hook 属于对 Agent 行为的外部干预点，与 [[agent-control-plane]]（权限/边界/审计）和 [[验证门禁化]]（未通过验证则强制阻断）同属"外部强制约束 Agent 行为"的方法族。