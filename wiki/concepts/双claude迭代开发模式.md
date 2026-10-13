---
type: concept
title: 双 Claude 迭代开发模式
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 方法论, 迭代, anthropic]
related: [writing-skills, skill-creator, 像做机器学习一样做prompt工程, 像做tdd一样做skill]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# 双 Claude 迭代开发模式

双 Claude 迭代开发模式被来源标注为"**最有效的 Skill 开发过程**"：由两个角色分离的 Claude 实例构成迭代循环——Claude A（专家）负责设计和优化 Skill，Claude B（测试者）用 Skill 执行真实任务，通过观察 B 的行为发现问题再回到 A 改进。

## 循环流程

```
Claude A（专家）帮你设计和优化 Skill
    ↓
Claude B（测试者）用 Skill 执行真实任务
    ↓
观察 Claude B 的行为，发现问题
    ↓
回到 Claude A 改进 Skill
    ↓
重复直到满意
```

## 方法论定位

与 [[skill-creator]] 的自动化 run_loop（ML 式评测）和 Writing-Skills 的 [[像做tdd一样做skill]]（测试先行）构成**第三条方法论路线**——人工观察迭代式。三者角色分离思想同源（设计者与评估者分离），但信息回路不同：自动化评测回路 vs 压力测试回路 vs 真实任务观察回路。适用边界来源未对比，待 wiki 侧综合裁决。

## 与 Grader 自评张力的关系

双 Claude 模式中"观察 Claude B 行为"的主体可以是人类或 Claude A，天然规避了单一 LLM 自评的可靠性问题（见 [[llm自我评估不可靠]]、[[grader自我批评机制]]）。
