---
type: comparison
title: "Skill-Creator 与 Writing-Skills 方法论对比"
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, writing-skills, 方法论对比, agent-skills, 评测]
related: [skill-creator, writing-skills, superpowers插件, anthropic, 像做机器学习一样做prompt工程, 像做tdd一样做skill, 双claude迭代开发模式, skill-creator六大局限, skill-creator三agent评估链, skill评测数据管道, 触发测试集, 合理化借口反驳, skill简洁性原则, skill生态知识体系]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill-Creator 与 Writing-Skills 方法论对比

**来源声明**：珂罗《Agent Skill规范、构建与设计模式》明示二者"目标相似，但方法论截然不同"，并在总结章将它们并置于 [[skill生态知识体系]] 的"构建方法论"层（Anthropic/Superpowers），但**全文未设正面对比章节**——本页属 wiki 侧综合，全部证据来自该单一来源。

## 逐维对比

| 维度 | Skill-Creator（ML 式评测路线） | Writing-Skills（TDD 式路线） |
|---|---|---|
| 形态定位 | Anthropic 官方元工具——"用来创建 Skill 的 Skill" | [[superpowers插件]] 框架中的元技能——教 Agent 如何创建新 Skill |
| 核心哲学 | [[像做机器学习一样做prompt工程]]：训练集/测试集、A/B 测试、量化基准、防过拟合 | [[像做tdd一样做skill]]：压力场景=测试用例、SKILL.md=生产代码、RED/GREEN/REFACTOR |
| 测试载体 | evals.json（prompt+expectations）、trigger_eval.json（should_trigger 正反例，见 [[触发测试集]]） | 压力场景（时间+沉没成本+疲劳组合施压）+ 子代理执行 |
| 评估机制 | [[skill-creator三agent评估链]]（Grader 8 步评分+自我批评 / Comparator 双盲 1-10 分 / Analyzer 双角色分析）+ Eval Viewer 人工评审 | 无量化评分；以"压力下是否守规"为通过标准，失败信号是 Agent 的合理化借口 |
| 迭代机制 | feedback.json 人工反馈回灌 + run_loop.py 自动训练/测试分割迭代 description（`--max-iterations 5`） | 观察 Agent 行为 → GREEN 只补观察到的失败 → REFACTOR 用 [[合理化借口反驳]] 逐一堵漏 |
| 数据管道 | 8 个 JSON 文件的评测数据流（见 [[skill评测数据管道]]） | 无独立数据管道，产物是 Skill 文档本身 |
| 成本/基础设施 | 高：3 用例=10 个子 Agent；Issue #514 成本爆炸；需 5 项前置知识（见 [[skill-creator六大局限]]） | 低：手写即可，社区称"简单需求直接手写 SKILL.md 快得多" |
| 核心优势 | 最系统化、量化基准可回归、触发准确率可自动优化 | 轻量、直击真实行为失败、零基础设施依赖 |
| 主要风险 | 成本/流程/并发/操作型失效/膨胀/学习曲线六重局限 | 依赖压力场景设计质量；缺量化基线，难以回归比较 |

## 深层同源性

两套方法论在底层共享"角色分离"与"以行为为评估对象"的思想：

- Skill-Creator 把"设计者/评分者/比较者/分析者"拆为独立子 Agent；Writing-Skills 把"设计者"（Claude A）与"测试执行者"（Claude B/子代理）分离——后者在 3.6 节被固化为 [[双claude迭代开发模式]]；
- 两者都拒绝只看输出文本：Skill-Creator 读 Transcript 与执行者笔记，Writing-Skills 观察压力场景下的实际选择；
- 两者都强调泛化与精简（防过拟合 ↔ [[skill简洁性原则]]）。

Skill-Creator 三大核心思想（泛化而非过拟合、解释"为什么"而非堆砌"必须"、提取重复模式）中，后两条在 Writing-Skills 中以 GREEN 最小化与借口反驳的形式隐性复现。

## 第三条路线与适用边界（开放问题）

本文实际并置了**三条**构建方法论：Skill-Creator 自动化评测（ML 式）、Writing-Skills 测试先行（TDD 式）、[[双claude迭代开发模式]]（人工观察迭代式，被称为"最有效的 Skill 开发过程"）。来源未裁决三者适用边界；可观察到的定位差异是：Skill-Creator 适合需要量化回归的复杂 Skill，Writing-Skills 适合纪律执行型 Skill 的快速开发，双 Claude 模式是两者的轻量近似。此裁决有待后续来源验证。
