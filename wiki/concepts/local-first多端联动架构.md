---
type: concept
title: Local-First 多端联动架构
tags: [openclaw, 架构设计, local-first, 控制平面]
related: [openclaw, gateway单一控制平面, openclaw-channel架构, agent-control-plane, clawhub]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---
# Local-First 多端联动架构

Local-First 多端联动是 [[openclaw]] 的总体设计核心：本地优先运行 + 多端联动 + 以 Gateway 为核心控制平面的分布式系统。OpenClaw 不是单一 Agent 程序，而是一个以 Gateway 为枢纽、连接通讯平台、设备节点、工具与技能生态的全栈系统。

## 五大架构组件

| 组件 | 定位 | 关键规格 |
|------|------|---------|
| Gateway | 统一控制平面 | 基于 WebSocket，Node ≥22 下 Daemon 常驻；管理 Sessions/Presence/配置/Cron/Webhooks/Control UI/Canvas 宿主 |
| Pi Agent | 智能体运行时核心引擎 | RPC 模式，支持 Tool Streaming + Block Streaming；按频道/账户/同伴路由到相互隔离的智能体（独立 Workspace 与会话） |
| Channels | 社交生态连接 | 原生支持 10 个通讯平台：WhatsApp、Telegram、Slack、Discord、Google Chat、Signal、iMessage、Microsoft Teams、Matrix、Zalo |
| Nodes & Apps | 设备节点化 | `node.invoke` 远程调用硬件（摄像头拍照/录码、屏幕录制、地理位置）；macOS `system.run`；Voice Wake & Talk Mode 基于 ElevenLabs 等 |
| Tools & Skills | 工具与技能 | 托管 Chrome/Chromium 浏览器控制、Live Canvas（A2UI 画布）、[[clawhub]] 技能注册表 |

## 安全与部署设计

安全动因：OpenClaw 会连接真实的社交媒体和本地文件系统。

- **DM 配对门控**：未知发送者必须通过配对码验证，bot 才处理其消息。
- **Docker 非主会话沙箱**：群组/外部频道会话放入独立容器运行，限制主机访问，敏感工具（浏览器/系统命令）走黑白名单。
- **部署弹性**：Gateway 可跑本地或小型 Linux 实例，经 Tailscale Serve/Funnel 或 SSH 隧道安全远程访问。

## 关联

- Gateway 详见 [[gateway单一控制平面]]；整体权限/边界/角色分离设计是 [[agent-control-plane]] 概念的具体实例。
- 作者将 OpenClaw 定位为"开启新的软件构建范式"——该评价属无口径主张，见 [[ai工程量化效果声明追踪]]。