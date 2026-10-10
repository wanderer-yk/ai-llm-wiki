---
type: entity
title: ClawHub
tags: [openclaw, 技能注册表, skill生态]
related: [openclaw, 生产级skill, skill-command-mcp三层架构, 渐进式披露替代向量检索]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---
# ClawHub

ClawHub 是 [[openclaw]] 的官方技能注册表平台，支持捆绑技能、托管技能、工作区技能三类技能的自动搜索与安装（来源：《深入理解OpenClaw技术架构与实现原理（上）》总体架构章"Tools & Skills"节）。它是 OpenClaw 技能生态的核心命名子系统，与 Gateway 内置的工具系统、工作区技能共同构成 OpenClaw 的能力扩展面。

## 在 OpenClaw 架构中的位置

ClawHub 出现在总体架构第 5 节"工具与自动化：Tools & Skills"，与托管浏览器控制、Live Canvas（基于 A2UI 的实时交互画布）并列介绍。三类技能的划分：

| 类型 | 说明 |
|------|------|
| 捆绑技能 | 随 OpenClaw 发行包捆绑 |
| 托管技能 | 由 ClawHub 平台托管分发 |
| 工作区技能 | 存放于用户工作区的本地技能 |

## 待核证项

本文上篇未展开 Skills 模块（16 模块地图中的模块 10，预期归属下篇）。ClawHub 三类技能与既有概念 [[生产级skill]]、[[skill-command-mcp三层架构]] 的具体对应关系、技能审核与质量管控机制，均待下篇或官方文档补证。