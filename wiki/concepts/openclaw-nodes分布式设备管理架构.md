---
type: concept
title: OpenClaw Nodes 分布式设备管理架构
tags: [openclaw, 分布式, 设备管理, 安全]
related: [openclaw, gateway单一控制平面, 多agent系统工程论]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603260830]深入理解OpenClaw技术架构与实现原理下.html"]
---
# OpenClaw Nodes 分布式设备管理架构

Nodes 是 OpenClaw 把多台设备（手机/桌面）接入 Gateway 控制平面的分布式执行层（3.14 节）。

## 配对流程（七步人工审批制）

`node.pair.request → pending 列表 → 广播 node.pair.requested 通知管理员 → 人工审批 → node.pair.approve 生成 token → node.pair.verify 验证 → 连接建立`

## 命令执行三重前置

allowlist → 节点声明 commands → 敏感命令（camera.snap/screen.record/sms.send）额外配置（gateway/node-command-policy.ts:56）。

## 三级唤醒

maybeWakeNodeWithApns（等 3s）→ 重试（等 12s）→ maybeSendNodeWakeNudge 可见提醒。

## Node Host 三模块

runner.ts（WebSocket 客户端、声明 caps/commands）、invoke.ts（system.run/which/browser.proxy、执行审批）、config.ts（`~/.openclaw/node.json`）。

## 安全五机制

token+节点 ID 双验、命令白名单、Exec 审批、sanitizeHostExecEnv 防 PATH 污染、200KB 输出截断。

缺口：3.14.5 命令分类表与 3.14.7 事件类型表为图片不可提取。与 [[gateway单一控制平面]] 构成「控制面/执行面分离」——企业级控制平面思路的个人版先声。
