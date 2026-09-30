---
type: entity
title: AoneSandbox
created: 2026-09-30
updated: 2026-09-30
tags: [阿里, 沙箱, aone, agent]
related: [trade-spec, aone, aone-agent, claude-agent-sdk, agent-control-plane, agent-canvas沙盘机制]
sources: ["[202603301537]从VibeCoding到范式编程用Spec打造淘系交易的AI领域专家.html"]
---

# AoneSandbox

AoneSandbox 是阿里内部的沙箱平台，在《从 Vibe Coding 到范式编程》中被[[交易业务技术团队]]选为 [[trade-spec]] 沙箱 MVP 部署的底层承载。其选型核心理由是「安全隔离与网络互通的平衡」：一方面 SOTA 模型作为 AI Agent 会动态执行代码、存在安全风险，且集团生产网明确禁止运行沙箱类服务；另一方面项目需要双向网络访问——既访问公网调用 API，又访问弹内测试网的代码平台（git 仓库拉取、分支操作）和内部 MCP 服务（变更创建、代码评审等）。

## 网络能力与使用方式

- **弹内测试网模式**：「既支持安全外联访问互联网，又与集团测试网互通可以访问内部产研平台」，同时满足安全外联与内网互通两点需求；沙箱 MVP 因此部署于测试网环境而非生产网。
- **Python 适配层**：使用沙箱需部署 Python 适配层，完成任务提交、拉取仓库、回调三类操作。
- **与 Hooks 的绑定**：Sandbox 生码结束后执行 Hooks 回调服务端，与平台「Hooks 事件驱动机制」同原理。

## 关联与开放问题

- 与 [[aone]]/[[aone-agent]] 同属 Aone 家族，但是否独立产品及家族边界待核验。
- 与 [[agent-control-plane]]（权限/边界/审计）、[[agent-canvas沙盘机制]]（沙盘隔离不可逆动作）、[[coding-agent四特征]]（封闭层）在概念上同域。
- 开放问题：ACK（「ideaTalk 相关的 ACK 信息」）具体含义；Step3 之后的部署与生产化权限治理（Demo 中 `permission_mode='bypassPermissions'` 与安全底线论述的收敛方式）。