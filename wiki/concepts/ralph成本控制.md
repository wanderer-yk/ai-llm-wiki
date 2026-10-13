---
type: concept
title: Ralph 成本控制
tags: [成本, 迭代上限, token限制, ralph-loop]
related: [ralph-loop, hitl到afk渐进路径, agent可观测性六维度, 机器可验证完成条件, docker沙箱隔离]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# Ralph 成本控制

Ralph 成本控制是运行 Ralph Loop 的经济性约束体系，核心铁律是"始终设置 `--max-iterations` 保护你的钱包"——因为 Ralph 循环可能运行数小时甚至几十个小时，成本敏感场景被明确列为不适用场景。

## 量化成本分级（基准：Claude 3.5 Sonnet）

| 任务规模 | 迭代次数 | 成本估算 |
|---|---|---|
| 小任务 | 5-10 迭代 | $5-15 |
| 中等任务 | 20-30 迭代 | $15-50 |
| 大型任务 | 30-50 迭代 | $50-150 |

## 影响因素

1. 代码库大小（影响上下文窗口消耗）
2. 任务复杂度（影响迭代数）
3. 模型选择（GPT-4 vs Claude vs 本地模型）

## 五条策略（技巧 10）

1. HITL 起步，再转 AFK（[[hitl到afk渐进路径]]）
2. 始终设置 `--max-iterations`（如 `--max-iterations 20`）
3. 选成本效益最优任务：机械化重构、测试迁移，避免创意性任务
4. 本地模型（如 Llama 3.1）用于简单预处理；文章判断本地模型在复杂代码任务上表现仍有差距
5. 投资回报视角：几小时替代数天工作，则 $50-150 值得

## 三条策略（实践建议章）

1. 结合成本监控和 token 使用限制策略
2. 优先使用 HITL 模式学习和优化提示
3. 仅在提示稳定后使用 AFK 模式

完整命令示例：`/ralph-loop "Task description" --max-iterations 30 --completion-promise "DONE"`

## 跨来源关联

成本是 [[agent可观测性六维度]] 中 cost 维度的实践落地；迭代上限与 [[机器可验证完成条件]] 构成循环退出的双保险（完成承诺 OR 达到上限）。