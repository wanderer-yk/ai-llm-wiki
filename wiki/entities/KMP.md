---
type: entity
title: KMP
created: 2026-10-09
updated: 2026-10-09
tags: [Kotlin, 跨平台, 代码迁移]
related: [网盘, 三层架构管住AI输出, 两阶段迁移流程, AI迁移三规律性问题]
sources: ["[202605201800]网盘存量代码迁移实战我们如何用三层架构管住AI的输出.html"]
---
# KMP（Kotlin Multiplatform）

KMP（Kotlin Multiplatform，Kotlin 多平台）是 JetBrains 推出的基于 Kotlin 的跨平台技术，允许在 Android、iOS 等多端之间共享业务逻辑等公共代码、同时保留各端原生能力——此为独立可验证的公开背景信息，非源文内容。

在本文语境中，KMP 是 [[网盘]] 存量代码迁移的目标技术：团队"推进 KMP 多端复用"，需要把 Android 主端积累的大量存量页面代码迁移为 KMP 结构。迁移涉及 UI 层、布局文件、业务逻辑、资源文件四类内容，核心挑战被重新定义为"迁得稳不稳"而非"能不能迁"（[[AI迁移三规律性问题]]）。

## 在本文中的角色

KMP 迁移是 [[三层架构管住AI输出]] 的实践载体：提取阶段由 Agent Team 对四类文件并行提取，转化阶段按"模块生成 → 资源转化 → 业务代码迁移 → UI 转化"串行推进（[[两阶段迁移流程]]）。