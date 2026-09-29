---
type: entity
title: GitLab Webhook
created: 2026-06-25
updated: 2026-06-25
tags: [gitlab, webhook, event-driven, ci-cd, automation]
related: [业务级code-review, diff预处理流水线, 货拉拉大前端]
sources: ["[202602271400]如何用AI做业务级CodeReview.html"]
---
# GitLab Webhook

## 概述

GitLab Webhook 是 GitLab 提供的事件驱动回调机制，允许在特定仓库事件发生时向预设的 URL 发送 HTTP POST 请求，从而触发外部系统的自动化流程。

## 支持的事件类型

在 [[货拉拉大前端]] 的 AI Code Review 系统中，GitLab Webhook 主要监听两类事件：

- **Push events**：代码推送到分支时触发
- **Merge request events**：合并请求创建或更新时触发

## 在 AI Code Review 中的应用

货拉拉 AI Code Review 系统以 GitLab Webhook 作为整个流水线的入口。当开发者执行 `git push` 或提交 Merge Request 时，Webhook 回调携带 Diff 数据进入 [[diff预处理流水线]]，启动后续的 RAG 召回、深度 Review 和报告生成流程。

这种事件驱动设计确保了审查流程的自动化和无侵入性——开发者无需改变原有工作习惯，系统在代码推送的瞬间自动启动审查。

## 相关概念

- [[业务级code-review]]：货拉拉 AI 代码评审系统的整体设计理念
- [[发布卡点校验]]：未来计划将审计结果接入发布流水线作为标准门控条件