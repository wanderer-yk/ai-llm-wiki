---
type: concept
title: Agent 自举三前提
tags: [agent, 自举, 基础设施, sdd]
related: [zhiyuanfu, 24h打工人, 自举式开发, sdd留痕进化论]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# Agent 自举三前提

Agent 自举不是凭空发生的，需要三项基础设施才能让系统修复自己的 bug：

1. **清晰设计文档** — AI 知道系统边界和设计意图
2. **SDD 标准流程** — spec→plan→tasks 的完整转化链路
3. **constitution.md** — 架构约束（目录结构/模块边界/命名规则），让 AI"在框架内工作而非自由发挥"

## 真实案例

[[24h打工人]] 系统通过自身反馈系统提交 RadioGroup 组件 onChange 未绑定的 bug → 触发 SDD 流程 → 自动定位修复 → 企微通知完成。

## 与已有概念关联

为 Wiki 已有的 [[自举式开发]] 概念提供了更具体的前提条件和真实案例。与 [[活文档机制]] 的规范文档理念互补。