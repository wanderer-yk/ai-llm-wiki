---
type: concept
title: Skill-Creator 六大局限
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, agent-skills, 局限分析, 成本控制, 触发机制]
related: [skill-creator, anthropic, 操作型skill触发失效, skill膨胀风险, 评估任务模型降配, skill-creator三agent评估链, 像做机器学习一样做prompt工程, 触发测试集, skill-creator-vs-writing-skills]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill-Creator 六大局限

Skill-Creator 六大局限指 Anthropic 官方 Skill 元工具 [[skill-creator]] 在社区使用与作者分析中被系统记录的六项已知问题，出自珂罗《Agent Skill规范、构建与设计模式》2.7.2 节"已知局限与社区反馈"。这些局限不否定其"像做机器学习一样做 Prompt Engineering"方法论的价值，而是划定了该工具的适用边界，并构成 [[像做tdd一样做skill]]（Writing-Skills TDD 路线）与 [[双claude迭代开发模式]]（人工观察迭代路线）等替代方法论的动机。

## 问题一：Token 消耗极高、成本不透明

GitHub Issue #514（anthropics/claude-plugins-official 仓库，报告者 jroy-poka，2026-03-04）给出实证："20 个评估查询 × 每条 3 次运行 = 60 个会话，消耗约 69% 的 5 小时配额，0 条可执行结果"。

- **根因**：SKILL.md 第 385 行指示 `run_loop.py` 使用 `--model <session-model>` 透传机制，触发检测继承主会话的 Opus 级模型，产生 60+ 个 Opus 级 `claude -p` 子进程。而触发检测本质是"是/否"二元信号，无需高阶推理能力（详见 [[评估任务模型降配]]）。
- **社区修复建议**：eval 默认模型改为 claude-haiku，可降低成本 10-20 倍且触发检测精度等价；截至成文 Issue #514 仍处 Open 状态，上游未采纳。
- **认识论张力**：该实证与作者 2.7.1 自评的"评估体系严谨"构成自我披露张力（"评估严谨" vs "0 actionable results"），作者主动并置两条证据，属良好认识论实践，已登记 [[ai工程量化效果声明追踪]] 作正例。

## 问题二：流程冗长

完整使用流程包含 9 个交互确认节点：

```
需求确认 → Skill 草稿确认 → 测试用例确认 → 并行运行（等待）
→ 断言起草确认 → 评审 Viewer → 反馈提交 → 改进确认 → 再次运行
```

每轮迭代用户需 4 步手动操作：逐个查看输出 → 撰写反馈 → 提交 feedback.json → 回对话告知 Claude。该项与 2.7.1 自评优势"人机协作设计（Eval Viewer 分工）"构成第二组自披露张力——同一 HITL 设计的强弱点并置。

## 问题三：子任务并发复杂

单轮完整评测的子 Agent 数量公式为 **N×2 + N + 1 + 可选 N**（执行 N×2、Grader N、Analyzer 1、Comparator 可选 N）；3 个测试用例即产生 10 个子 Agent，多轮迭代线性增长，且受 [[skill-creator]] 所依赖的 Claude Code 子 Agent 并发上限约束，容易排队等待。与 [[skill-creator三agent评估链]] 的角色设计直接相关。

## 问题四：操作型 Skill 触发失效

Issue #514 的深层发现："operational workflow skills show 0% recall regardless of description quality"——"运行部署脚本""生成日报"类操作型 Skill 触发率恒为 0%，description 优化完全无效。触发与否取决于任务复杂度/专业性而非 description 质量，是 [[模型驱动触发逻辑]] 的边界条件，详见 [[操作型skill触发失效]]。

## 问题五：Skill 膨胀

迭代改进不断累积指令、示例与边界处理，导致 Skill 体积失控（Medium《Claude Code Skills Deep Dive》观察："A 5KB skill balloons to 50KB"），违背"保持精简"初衷。详见 [[skill膨胀风险]]。

## 问题六：学习曲线陡峭

完整使用 Skill-Creator 需理解 5 项知识领域：三层渐进式加载（[[skill渐进式加载机制]]）、7 种 JSON Schema（[[skill评测数据管道]]）、子 Agent 原理（[[skill-creator三agent评估链]]）、触发率统计（[[触发测试集]]）、防过拟合分割（[[像做机器学习一样做prompt工程]]），非技术用户认知负担高。社区观点："对于简单需求，直接手写 SKILL.md 比用 skill-creator 快得多。"

## 定位与影响

六大局限共同指向一个结论：Skill-Creator 适合"有量化评测需求、能承担基础设施成本"的复杂 Skill 场景，而轻量场景可改用 [[像做tdd一样做skill]]（TDD 路线）或 [[双claude迭代开发模式]]（人工观察迭代路线）。三条方法论路线的并置对比详见 [[skill-creator-vs-writing-skills]]。
