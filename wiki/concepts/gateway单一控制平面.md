---
type: concept
title: Gateway 单一控制平面
tags: [openclaw, gateway, 控制平面, websocket, 协议]
related: [openclaw, agent-control-plane, local-first多端联动架构, openclaw定时任务系统, openclaw-channel架构]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---
# Gateway 单一控制平面

Gateway 是 [[openclaw]] 的"心脏"与统一控制平面：本质为一个 WebSocket 服务器，作为守护进程（Daemon）常驻后台（推荐 Node ≥22），为所有客户端、工具和事件提供统一连接通道。它是 [[agent-control-plane]] 概念（权限/边界/审计）在 OpenClaw 中的具体实现。

## 五大职责

1. 消息路由（Telegram/Discord/Slack 等全频道）
2. 会话生命周期管理
3. 工具调用协调
4. iOS/Android 节点通信桥接
5. OpenAI 兼容 REST API

同时管理 Sessions、Presence、配置、Cron、Webhooks、Control UI 与 Canvas 宿主。

## 关键特性

| 特性 | 说明 |
|------|------|
| 单端口复用 | WebSocket RPC + HTTP API + Control UI 共用一个端口（默认 18789） |
| 协议版本化 | 客户端声明 `minProtocol/maxProtocol`，服务端拒绝不匹配的连接 |
| 角色分离 | `operator`（控制面）和 `node`（能力节点）两种角色 |
| 作用域控制 | 细粒度 scopes（`operator.read`、`operator.write`、`operator.admin` 等） |
| 设备认证 | 支持设备身份验证和配对机制 |
| 热重载 | 四档：`off`/`hot`/`restart`/`hybrid`（`hybrid` 默认，配 `debounceMs: 300` 防抖） |

## 协议细节

帧类型（原文）：

```text
Request:  {type:"req", id, method, params}
Response: {type:"res", id, ok, payload|error}
Event:    {type:"event", event, payload, seq?, stateVersion?}
```

连接握手四步：`connect.challenge`（可选 nonce 挑战）→ `connect`（auth + role + scopes）→ `hello-ok`（policy + 设备令牌）→ `events`（持续推送）。

## 认证、绑定与安全强制

| 认证模式 | 场景 |
|----------|------|
| `token` | 共享令牌认证（默认） |
| `password` | 共享密码认证 |
| `trusted-proxy` | 反向代理认证（如 Pomerium） |
| `device-token` | 设备身份认证（配对后自动获取） |

绑定五模式：`loopback`（127.0.0.1，默认）/`lan`（0.0.0.0）/`tailnet`/`auto`/`custom`。安全强制为硬性规则：非环回地址绑定必须启用认证；明文 `ws://` 禁止连接非本机地址（作者标注对应 CWE-319）。

## 运维与源码地图

服务生命周期跨平台：macOS launchd（`openclaw gateway install/start/stop/restart`）+ Linux systemd user service（`openclaw-gateway.service`）。常用命令：`gateway --port`（启动）、`gateway status [--deep]`、`gateway health` + `channels status --probe`、`gateway discover`（局域网发现）、`logs --follow`。

核心源码五路径：CLI 入口 `src/cli/gateway-cli/`、客户端 `src/gateway/client.ts`、协议定义 `src/gateway/protocol/`、服务端 HTTP `src/gateway/server-http.ts`、配置类型 `src/config/types.gateway.ts`。