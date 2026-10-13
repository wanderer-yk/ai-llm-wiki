---
type: concept
title: HITL 到 AFK 渐进路径
tags: [运行模式, hitl, afk, 渐进采纳, ralph-loop]
related: [ralph-loop, human-in-the-loop, human-in-the-loop测试sop, ralph成本控制, docker沙箱隔离, 24h打工人, ai编程四阶段演进]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601270830]从ReAct到RalphLoopAIAgent的持续迭代范式.html"]
---

# HITL 到 AFK 渐进路径

HITL 到 AFK 渐进路径是 Ralph Loop 的采纳方法论：先以 HITL（Human-In-The-Loop，人在回路）模式运行循环——类似结对编程，是学习 Ralph、优化提示词、建立信心的最佳方式；提示稳定后再切换到 AFK（Away From Keyboard）模式——长时间运行、自主、无人值守的编程；最后返回人工审查提交结果。

## 三步进展

1. **HITL 学习**：观察循环行为，迭代优化提示（成本更可控）
2. **转 AFK**：提示稳定后无人值守运行
3. **返回审查**：人工审查 AFK 产出的提交

## AFK 安全铁律

"始终限制迭代次数——随机系统的无限循环是危险的。"AFK 循环配套 [[docker沙箱隔离]]（文章定位：HITL 沙箱可选、AFK 过夜循环是"防止失控代理的基本保险"）与通知机制（CLI / 邮件 / 消息推送）。

## 量化特征

- 典型循环时长：30–45 分钟
- 可持续运行：数小时（成本敏感场景警告：甚至几十小时，见 [[ralph成本控制]]）
- 迭代次数建议：小任务 5-10 / 中等任务 20-30 / 大型任务 30-50

## 文中互证

该渐进路径在来源文章中出现四处（技巧 2、技巧 10、实践建议-安全机制和资源控制、场景适用性），全部互证一致，是全文一致性最强的主张之一。

## 跨来源对照

- [[human-in-the-loop]]（AI 狼人杀的人机对局机制）：同为"人保留关键节点控制权"的机制
- [[human-in-the-loop测试sop]]（美团）：人工主导 AI 辅助的测试流程，同为渐进信任结构
- [[24h打工人]]（zhiyuanfu）：无人值守系统的另一实现，可作为 AFK 范式的独立工程验证