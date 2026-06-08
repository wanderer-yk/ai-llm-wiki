---
type: concept
title: 跨厂商模型对抗审核
tags: [code-review, ai, 模型对抗, 质量]
related: [gao-jie-mo-xing-shen-cha-di-jie-mo-xing, pre-pr-ji-zhi, quality-inspection-ai]
created: 2026-06-08
updated: 2026-06-08
sources: ["用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# 跨厂商模型对抗审核

不同厂商的模型互相审查对方生成的代码，通过能力差异化形成互补，实测CR覆盖面更全。是[[gao-jie-mo-xing-shen-cha-di-jie-mo-xing|高阶模型审查低阶模型]]模式的扩展。

## 核心思路

单一模型的盲区是系统性的，但不同厂商模型的盲区往往不同。通过"对抗"式交叉审查：

- 厂商A的模型审查厂商B模型生成的代码
- 厂商B的模型审查厂商A模型生成的代码
- 两份审查结果合并，覆盖面显著提升

## 关联

- 与[[quality-inspection-ai|质检AI]]属于同领域不同应用
- 是[[pre-pr-ji-zhi|Pre-PR机制]]中AI辅助CR环节的具体技术手段