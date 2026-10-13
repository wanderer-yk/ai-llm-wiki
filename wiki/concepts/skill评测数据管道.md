---
type: concept
title: Skill 评测数据管道
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, 评测, 数据管道, json]
related: [skill-creator三agent评估链, 触发测试集, agent评测四要素, 像做机器学习一样做prompt工程]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 评测数据管道

Skill 评测数据管道是 [[skill-creator]] 评测基础设施的数据结构层：由 `references/schemas.md` 定义的一系列 JSON 数据结构，串联从测试定义到版本追踪的完整评测数据流。与 [[skill-creator三agent评估链]]（Agent 分工层）互补——一个讲数据流，一个讲角色分工。

## 管道流程

```
evals.json          ─── 测试定义（prompt + expectations）
    ▼
timing.json         ─── 运行计时（来自子 Agent 完成通知）
    ▼
metrics.json        ─── 执行指标（工具调用次数、文件数等）
    ▼
grading.json        ─── 评分结果（断言通过/失败 + 证据）
    ▼
benchmark.json      ─── 聚合基准（mean ± stddev，delta 对比）
    ▼
comparison.json     ─── 盲比较结果（A/B 评分 + 赢家）
    ▼
analysis.json       ─── 事后分析（改进建议 + 执行模式洞察）
    ▼
history.json        ─── 版本追踪（迭代历史 + 当前最佳）
```

## 关键结构

- **evals.json**：`{"skill_name": "code-review", "evals": [{"id": 1, "prompt": "...", "expected_output": "..."}]}`——测试用例定义
- **feedback.json**：`{"reviews": [{"run_id": "eval-1-java-npe-with_skill", "feedback": "", "timestamp": "..."}], "status": "complete"}`——人工评审输入；run_id 命名（3 案例 × with/without_skill = 6 条）证实测试设计
- **benchmark.json**：聚合基准含 mean ± stddev 与 delta 对比

## 待核计数出入

正文称"7 种 JSON 数据结构"，管道图列出 8 个文件（evals/timing/metrics/grading/benchmark/comparison/analysis/history）——history.json 是否在 schemas.md 之外待核实（登记 [[ai工程量化效果声明追踪]]）。

## 关联

与 [[agent评测四要素]]（评测对象/数据集/指标/反馈闭环）跨来源同构；Analyzer 角色 B 对该管道聚合数据的模式分析（无区分力断言/高方差断言/异常值）见 [[skill-creator三agent评估链]]。
