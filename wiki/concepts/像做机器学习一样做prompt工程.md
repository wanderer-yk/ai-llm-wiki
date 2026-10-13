---
type: concept
title: 像做机器学习一样做 Prompt Engineering
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, 方法论, 机器学习, prompt-engineering]
related: [skill-creator, skill评测数据管道, 像做tdd一样做skill, 双claude迭代开发模式, agent评测四要素]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# 像做机器学习一样做 Prompt Engineering

"像做机器学习一样做 Prompt Engineering"是 [[skill-creator]]（Anthropic 官方元工具）的设计哲学：将 ML 工程实践——训练集/测试集分割、评估指标、迭代优化循环、防过拟合机制——移植到 Skill（Prompt 工件）的开发中，同时移植 CI/CD、A/B 测试、性能基准等软件工程最佳实践。

## ML 框架到 Skill 开发的映射

| ML 概念 | Skill 开发对应物 |
|---|---|
| 训练集/测试集 | evals.json 测试定义 + 训练/测试集分割 |
| 评估指标 | 量化断言 + benchmark.json 聚合基准（mean ± stddev） |
| 迭代优化循环 | feedback.json 人工反馈回灌 → 改进 → 重跑（Step 6） |
| 超参自动调优 | run_loop.py 自动迭代优化 description（`--max-iterations 5`） |
| 防过拟合 | 泛化改进方向、避免针对单测试用例定制 |
| 消融实验 | with_skill / without_skill 并行 A/B 子 Agent |

## 机制级证据

run_loop.py 自动进行训练/测试集分割并迭代优化 description 触发准确率（[[触发测试集]] 作为评估集）；三 Agent 评估链（[[skill-creator三agent评估链]]）与 JSON 数据管道（[[skill评测数据管道]]）构成评测基础设施。作者自评其优势为"方法论完整——是目前最系统化的 Skill 开发框架"。

## 方法论三角

与 Writing-Skills 的 [[像做tdd一样做skill]]（TDD 式）和 [[双claude迭代开发模式]]（人工观察迭代式）构成三条 Skill 开发方法论路线；来源未做正面对比，适用边界待裁决。评估思想与 [[agent评测四要素]]、[[评测集优先于知识库]] 跨来源呼应。
