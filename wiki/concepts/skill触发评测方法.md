---
type: concept
title: Skill 触发评测方法
tags: [评测, 召回率, 精确率, description]
related: [skill评测集构建规范, skill评测三原则, description触发准确性权衡, agent欠触发倾向, 负向触发说明]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill 触发评测方法

Description 评测的全流程，核心问题是："**该用时用了吗？不该用时没用吧？**"

## 前置事实

见 [[agent欠触发倾向]]：Agent 基于自我能力评估决定是否找 Skill；Agent 天生偏向欠触发，Description 要主动外推边界。

## 评测集构建

16-20 条、分两组（应触发 8-10 条 / 不应触发 8-10 条），构建规范详见 [[skill评测集构建规范]]。

## 双指标

- **召回率**：应触发组中实际触发的比例。
- **精确率**：不应触发组中正确未触发的比例。

## 调试技巧

直接问 Agent"你什么时候会使用 [skill-name]"，根据其复述 Description 的内容判断理解偏差。

## 迭代改进决策规则（逐字保留）

| 失败模式 | 调整方向 |
|---|---|
| 漏触发居多 | 补充更多触发关键词和场景描述，把边界推得更宽 |
| 误触发居多 | 增加负向说明（"不要用于…"），收窄适用范围 |
| 两者都有 | Description 定位模糊，重新理清 Skill 核心边界 |

## 过拟合警告

修改 Description 后必须用**完整评测集**重跑对比；只修补失败 case 会过拟合——Description 面对的是无穷的真实 query。本方法属 [[skill评测三原则]] 中"分层评测"的 Description 层；Body 层见 [[skill-body评测对照实验]]。证据：来源含 code-review 与 less-to-postcss 两个 Skill 的触发测试实测截图。
