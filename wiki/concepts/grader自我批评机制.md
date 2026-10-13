---
type: concept
title: Grader 自我批评机制
created: 2026-10-13
updated: 2026-10-13
tags: [skill-creator, 评估, 自我批评, llm自评]
related: [skill-creator三agent评估链, llm自我评估不可靠, agent评测四要素]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Grader 自我批评机制

Grader 自我批评机制是 [[skill-creator]] 评估链中 Grader（评分者）的内省设计：Grader 在评估执行结果之后，还要**"评价评估本身"**——审查自己所用断言的质量，防止薄弱断言制造虚假信心。

## 核心原则

原文（英文+中译）："A passing grade on a weak assertion is worse than useless — it creates false confidence."（薄弱断言的通过比无用更糟——它制造虚假信心。）

## 断言审查三问

1. 通过的断言是否太易满足（如只查文件名存在而不查内容）？
2. 是否有重要结果未被任何断言覆盖？
3. 断言是否无法从可用输出验证？

配套规则：PASS 需要"真正任务完成"的证据；FAIL 需标注"巧合通过"（coincidental pass）情况。

## 与 LLM 自我评估不可靠的张力

"评价评估本身"本质上是 LLM 自评，与 Ralph Loop 来源的 [[llm自我评估不可靠]] 论点存在**张力待裁决**：Skill-Creator 的缓解设计包括盲比较（Comparator 双盲）、结构化断言、三角色分离——即不依赖单一自评，而以机制约束降低自评风险。该裁决对"何时可以信任 LLM 评估 LLM"这一更普遍问题（见 [[agent评测四要素]]）具有参照价值。
