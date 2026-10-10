---
type: concept
title: Gateway 协议与安全机制
tags: [openclaw, gateway, 协议, 认证, 安全]
related: [gateway单一控制平面, openclaw, agent-control-plane, local-first多端联动架构]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# Gateway 协议与安全机制

Gateway 协议与安全机制是 [[openclaw]] 统一控制平面（[[gateway单一控制平面]]）在连接层的字段级规范，涵盖连接握手、帧类型、认证模式、绑定模式、服务生命周期与配置热重载六个部分。来源：[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]] 3.1 节（3.1.4–3.1.9）的源码级拆解。

## 连接握手流程

```
Gateway                    Client
 │                          │
 │◄──── connect.challenge ──│  (可选：带 nonce 的挑战)
 │                          │
 │─────── connect (req) ───►│  携带 auth + role + scopes
 │                          │
 │◄────── hello-ok (res) ───│  返回 policy + 设备令牌
 │                          │
 │◄─────── events ──────────│  持续推送状态变更
```

`hello-ok` 返回的"设备令牌"与认证模式中的 `device-token` 为同一机制（高置信推断）。

## 帧类型

```text
Request:  {type:"req", id, method, params}
Response: {type:"res", id, ok, payload|error}
Event:    {type:"event", event, payload, seq?, stateVersion?}
```

Event 帧含可选 `seq` 与 `stateVersion` 字段，暗示存在有状态版本同步机制（待核证）。

## 认证四模式（3.1.5）

| 模式 | 使用场景 |
|------|----------|
| `token` | 共享令牌认证（默认） |
| `password` | 共享密码认证 |
| `trusted-proxy` | 反向代理认证（如 Pomerium） |
| `device-token` | 设备身份认证（配对后自动获取） |

## 安全强制规则

- 非环回地址绑定**必须**启用认证
- 明文 `ws://` 禁止连接非本机地址（作者标注对应 CWE-319：明文传输敏感信息）

## 绑定五模式（3.1.6）

| 模式 | 地址 | 用途 |
|------|------|------|
| `loopback` | 127.0.0.1 | 默认，仅本机访问 |
| `lan` | 0.0.0.0 | 局域网访问 |
| `tailnet` | Tailscale IP | Tailscale 网络 |
| `auto` | 自动选择 | 根据环境自动判断 |
| `custom` | 自定义地址 | 特定绑定需求 |

## 服务生命周期（3.1.7）

macOS（launchd）：

```bash
openclaw gateway install   # 安装 LaunchAgent
openclaw gateway start     # 启动服务
openclaw gateway stop      # 停止服务
openclaw gateway restart   # 重启服务
```

Linux（systemd user service）：

```bash
openclaw gateway install
systemctl --user enable --now openclaw-gateway.service
```

## 配置热重载四模式（3.1.8）

| 模式 | 行为 |
|------|------|
| `off` | 不重载 |
| `hot` | 仅应用安全热更新 |
| `restart` | 需要重启时自动重启 |
| `hybrid` | 安全时热更新，必要时重启（默认） |

配套 `debounceMs: 300` 防抖。记录修正：该节在源文前部曾被概括为"三模式"，3.1.8 表格补出 `off` 后确定为四档。

## 关键配置项（3.1.9）

```json
{
  "gateway": {
    "port": 18789,
    "bind": "loopback",
    "mode": "local",
    "auth": {
      "mode": "token",
      "token": "your-token"
    },
    "tls": {
      "enabled": true,
      "certPath": "/path/to/cert.pem",
      "keyPath": "/path/to/key.pem"
    },
    "reload": {
      "mode": "hybrid",
      "debounceMs": 300
    }
  }
}
```

## 关联

- [[agent-control-plane]]：`operator`（控制面）/`node`（能力节点）角色分离 + `operator.read`/`operator.write`/`operator.admin` 等 scopes 细粒度授权，是该概念在 Gateway 连接层的具体实例
- [[gateway单一控制平面]]：本页为其协议与安全维度的细节补充；默认端口 18789 在特性表、配置 JSON 与启动命令三处互证
- [[local-first多端联动架构]]：绑定五模式中的 `tailnet` 与 Tailscale Serve/Funnel 远程部署方案互补
