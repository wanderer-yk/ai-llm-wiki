---
type: concept
title: Skill 简洁性原则
created: 2026-10-13
updated: 2026-10-13
tags: [skill, 最佳实践, 上下文工程, tokens]
related: [skill工作流与反馈循环模式, skill自由度分级, skill膨胀风险, 上下文工程]
sources: ["[202605120830]AgentSkill规范构建与设计模式.html"]
---
# Skill 简洁性原则

Skill 简洁性原则是 Anthropic 官方最佳实践（经 writing-skills 转述）的第一原则：**context window 是公共资源**，Skill 写作应默认假设模型（Claude）已经聪明，只添加模型不知道的增量信息，不重复模型已具备的通用知识。

## 量化对比

来源给出 ~50 tokens（简洁版）vs ~150 tokens（冗余版）的对比示例：简洁版只陈述领域特定的增量规则，冗余版复述了模型本就知道的通用编程常识。约 3 倍的 token 差异在 Skill 被反复加载时会持续放大。

## 与膨胀风险的张力

该原则与 [[skill膨胀风险]]（迭代改进中指令/示例/边界处理累积导致 5KB→50KB）形成结构性对抗：Skill 有自然膨胀倾向，简洁性需要主动对抗——GREEN 最小化原则（[[像做tdd一样做skill]]：只为观察到的具体失败写 Skill，不为假设情况写）是其工程化保障。

## 关联

与 [[上下文工程]] 的整体目标（上下文是稀缺资源）一致；与 [[skill工作流与反馈循环模式]]、[[skill自由度分级]] 并列构成 Anthropic 官方最佳实践三支柱。
