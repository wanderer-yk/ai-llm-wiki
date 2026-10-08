---
type: concept
title: Skill Body 两种形态
tags: [body, 编写方法, 知识文档, 工作流]
related: [skill步骤间校验, skill-body评测对照实验, description三大要素, agent-skill, 分层模板体系]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill Body 两种形态

Description 只决定"在对的时间出现"，Body 质量才决定最终效果。来源按使用场景将 Body 分为两种形态，各有三要素：

## 1. 知识文档型

- **领域知识**：该场景下的专业背景与规则（范例：Code Review Standards，含按严重程度分级的问题清单）。
- **质量检查清单**：输出前逐项自查的标准。
- **Few-Shot 示例**：2-3 个好/坏对比示例。

## 2. 工作流型

- **步骤清晰**：明确编号的执行序列（范例：Sprint Planning Workflow 五步，其中故事点上限 = 平均速率 × 0.85 缓冲）。
- **步骤间校验**：每步带 Validation（见 [[skill步骤间校验]]）。
- **可循环迭代**：质量不达标可回退重做。

## 与其他概念的关系

Body 的详细知识应通过多层渐进下沉到 `references/`（[[skill渐进式披露]]），确定性逻辑下沉到脚本（[[skill脚本自动化原则]]）。Body 效果的验证方法是有/无 Skill 的对照实验（[[skill-body评测对照实验]]）。其"角色设定 + 通用规范 + 场景专属要求"的组织方式与转转 [[分层模板体系]] 在结构上同构。
