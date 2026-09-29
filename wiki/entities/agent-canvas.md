---
type: entity
title: Agent Canvas
tags: [amazon, agent, 沙盘, 可视化]
related: [agent-canvas沙盘机制, agent生产落地四层框架, openclaw]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604152000]OpenClaw落地到生产实际应用的一种可能的路径.html"]
---
# Agent Canvas

Amazon 在电商场景中构建的业务操作可视化画布/沙盘系统，由 vivo [[ding-junjie]] 在《[[sources/[202604152000]OpenClaw落地到生产实际应用的一种可能的路径|[202604152000]OpenClaw 落地到生产实际应用的一种可能的路径]]》中作为实证案例引用。

## 核心特征

- 将复杂、开放、不可逆的业务管理动作映射到可推演、可比较、可审查的画布空间。
- 不同版本具备类似 git 的版本管理特征，支持变更前后对比和多方案比较。
- Agent 不直接碰触真实生产系统，而是在画布中形成候选提案，经批准后才推进到真实执行。
- 本质是业务世界的"沙盘"，详见 [[agent-canvas沙盘机制]]。

## 在 Wiki 中的定位

Agent Canvas 是 [[agent生产落地四层框架]]（可视化/封闭/验证/回滚）在电商场景的实证案例，验证了 [[agent生产落地环境重构论]] 的可行性。