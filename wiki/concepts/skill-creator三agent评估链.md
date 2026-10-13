---
type: concept
title: Skill-Creator 三 Agent 评估链
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, 评估, 多agent, 角色分离]
related: [grader自我批评机制, skill评测数据管道, skill-creator, mr双重视角审查]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill-Creator 三 Agent 评估链

Skill-Creator 三 Agent 评估链是 [[skill-creator]] 的评测角色分工系统：由三个专业化子 Agent 构成，将"评分、比较、归因"三种评估职能分离，以角色分离对抗单一评估者的偏见。

## 三个角色

### Grader（评分者）
8 步流程：读 Transcript → 检查输出文件 → 评估断言 → 提取隐含声明 → 读执行者笔记 → **评价评估本身**（[[grader自我批评机制]]）→ 写结果 → 读指标数据。断言审查三问：通过的断言是否太易满足（如只查文件名存在不查内容）？是否有重要结果未被任何断言覆盖？断言是否无法从可用输出验证？

### Comparator（盲比较者）
借鉴医学双盲实验去偏见：对 with_skill/without_skill 两组输出做双盲盲比。双维度评分体系——内容（正确性/完整性/准确性，各 1-5 分）+ 结构（组织性/格式化/可用性，各 1-5 分），综合总分 1-10。判定优先级：总分 > 断言通过率 > 平局（极少出现）。

### Analyzer（分析者，双重角色）
- 角色 A（改进建议）：事后揭盲归因，输出分优先级 high/medium/low，类别分类为 instructions、tools、examples、error_handling、structure、references
- 角色 B（基准分析器）：分析聚合统计中的隐藏模式——哪些断言双配置 100% 通过（无区分力）、哪些高方差（不稳定）、时间/token 异常值

## 子 Agent 数量公式

| 组件 | 数量 |
|---|---|
| 执行子 Agent（with + without） | N × 2 |
| Grader | N |
| Analyzer | 1 |
| Comparator（可选） | N |

公式：**N×2 + N + 1 + 可选 N**；例：3 个测试用例 → 6 执行 + 3 评分 + 1 分析 = 10 个子 Agent。多轮迭代线性增长，受 Claude Code 子 Agent 并发限制易排队（[[skill-creator六大局限]] 问题三）。

## 关联

角色分离思想与 [[mr双重视角审查]]（代码+规划文档双视角）同源；Grader 自评环节与 [[llm自我评估不可靠]] 存在张力（见 [[grader自我批评机制]]）。
