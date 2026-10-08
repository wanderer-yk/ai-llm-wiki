---
type: concept
title: Description 触发准确性权衡
tags: [description, 触发, 误触发, 漏触发]
related: [description三大要素, 负向触发说明, agent欠触发倾向, skill触发评测方法, skill三阶段工作原理, 意图识别保守策略]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Description 触发准确性权衡

Description 是整个 Skill 体系**最关键的一行文字**：它是 Skill 的常驻索引，其精度直接决定 Skill 会不会在对的时间被加载。写不好会导致两种失败：

- **under-triggering（欠触发）**：该用时没触发——能力缺失（且是 Agent 默认倾向，见 [[agent欠触发倾向]]）。
- **over-triggering（过度触发）**：不该用时乱触发——浪费（见 [[负向触发说明]]）。

## 权衡的代价结构

Skill 激活本身消耗 1-2 步工具调用（见 [[skill三阶段工作原理]]），因此误触发 = Token 浪费与响应变慢，漏触发 = 能力缺失；description 精度直接影响 Token 消耗与响应速度。

## 编写规则

- 触发信息**必须且只能写在 Description**：写进 Body 无效，因为 Body 要触发后才加载。
- 好的 Description 同时回答三问（见 [[description三大要素]]）。

## 评测与调试

评测核心问题："该用时用了吗？不该用时没用吧？"——用召回率/精确率双指标度量（见 [[skill触发评测方法]]）。调试技巧：直接问 Agent"你什么时候会使用 [skill-name]"，据其复述 Description 判断理解偏差。

## 对照张力

本文主张 Description 要"主动把边界往外推"以对抗欠触发；有赞 [[意图识别保守策略]] 则主张"宁可让 AI 不回答，也不能让它答错"。二者层级不同（工具加载决策 vs 回答决策），作为对照素材而非矛盾。
