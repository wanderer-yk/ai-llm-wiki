---
type: concept
title: RD as QA
tags: [团队模式, 测试, ai-coding, 美团]
related: [美团技术团队,human-in-the-loop测试sop,pre-pr机制]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071958]用Agent评测思路管理AICoding31万行代码AI重构的实践.html"]
---
# RD as QA

## 定义

[[美团技术团队]]采用的团队组织模式：**100% 需求由研发（RD）兼任测试（QA）**，无独立 QA 角色。

## 背景

在 AI Coding 90%+ 代码自动生成的环境下，传统 QA 角色的价值定位发生变化。团队选择让研发直接承担质量保证责任。

## 配套实践

- [[human-in-the-loop测试sop]]：五步法人机协作测试流程
- [[pre-pr机制]]：AI 前置自查确保基础代码质量
- 测试路线选择：否决 AI 全自动测试（路线A），采用人工主导 AI 辅助（路线B）

## 核心洞察

- AI 全自动测试不可行：缺乏全局业务认知、极度依赖 PRD 质量、漏隐性关联高危场景、发散大量无价值边缘用例
- 测试环节反而需要比编码环节**更强的人工主导**——这与 AI Coding 90%+ 的高自动化比例形成鲜明对照