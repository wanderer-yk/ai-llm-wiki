---
type: concept
title: 三重猜测问题
tags: [harness-engineering, agent协作, ai编程幻觉]
related: [harness-engineering, ai-bian-cheng-huan-jue, cong-prompt-dao-harness, qian-hou-duan-san-ceng-jia-gou]
created: 2026-06-08
updated: 2026-06-08
sources: ["别让AI瞎猜了用HarnessEngineering终结无限返工.html"]
---
# 三重猜测问题

仅靠自然语言描述任务时，agent被迫同时猜测三件事，导致输出质量不可控。

## 三重猜测

1. **页面外观**：最终呈现是什么样？（设计/布局/交互）
2. **状态集合**：有哪些运行状态？（Default/Empty/Loading/Error/权限态/反馈态）
3. **代码拆分**：怎么组织代码？（组件层级/模块划分/路由结构）

## 累积偏移效应

"第一次也许能猜中，第二次、第三次就开始偏。"——猜测式开发存在累积偏移效应，每次基于不完整信息的微小编差会随迭代放大。

## 根因分析

前端场景：设计稿/状态演示/真实页面"三叉分叉"，迭代中三者慢慢分离
后端场景：一句话任务只说清目标，未说清边界（运行模式/IO/失败处理/验证/记录）

## 解决方案

[[harness-engineering|Harness Engineering]]通过三层架构直接消除猜测：
- **执行依据层**（如Pencil/docs）→ 消除外观猜测
- **状态暴露层**（如Storybook/runbook）→ 消除状态猜测
- **交付实现层** → 在前两层稳定后，agent角色变为"在既定结构和状态之上补实现/细节/接线"

## 与AI编程幻觉的关系

三重猜测是[[ai-bian-cheng-huan-jue|AI编程幻觉]]的具体结构性成因之一。当工程条件（结构/状态/边界）未提前冻结时，agent被迫猜测，产生看似合理但实际偏离预期的输出。