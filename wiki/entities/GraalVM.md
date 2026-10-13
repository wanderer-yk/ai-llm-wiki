---
type: entity
title: GraalVM
tags: [graalvm, 沙箱, 运行时, 多语言, code-as-action, 安全执行]
related: [AssistantAgent, spring-ai-alibaba, code-as-action, docker沙箱隔离]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html"]
---
# GraalVM

GraalVM 在本文语境中是 [[AssistantAgent]] 用作 AI 生成代码安全执行底座的**多语言沙箱运行时**：Assistant Agent 的技术特性声明"AI 生成的代码在 GraalVM 多语言沙箱中安全运行，具备资源隔离能力"，项目结构中 `assistant-agent-core` 模块内含"核心引擎：GraalVM 执行器、工具注册表"；文章致谢三项依赖的第三项即 GraalVM，官方 URL 为 `https://www.graalvm.org/`。

（独立可验证背景：GraalVM 是 Oracle 主导的开源高性能运行时与 JDK 发行版，支持 Java、JavaScript、Python 等多语言互操作，并以受限沙箱方式执行不可信代码，官网 graalvm.org。）

## 在 Code-as-Action 中的角色

在 [[code-as-action]] 范式下，Agent 生成的代码需要在运行时真实执行，GraalVM 承担该执行的安全边界角色：多语言支持（框架生成的是 Java 代码，沙箱具备多语言能力）+ 资源隔离。这是 Assistant Agent 与纯 function-calling 型 Agent 的关键差异之一——代码执行的可行性依赖沙箱安全底座。

## 证据状态与待验证问题

- **重要缺口**：GraalVM 沙箱的执行细节（权限模型、资源限额、可执行语言集合等）在全部 9 个 chunk 中均未以文字出现，可能仅存在于不可提取的架构图中——本页相关描述标注**待外部验证**。
- core 执行模块在原文核心模块介绍中始终未出现文字专节，仅由技术特性提及（chunk 5）与正式致谢（chunk 9）佐证其存在。

## 比较候选

- 与 [[docker沙箱隔离]]（OpenClaw 系实践的容器级隔离方案）构成两种 AI 代码执行沙箱路线：GraalVM 多语言沙箱（语言运行时级隔离）vs Docker 容器（操作系统级隔离），可做跨框架比较。
