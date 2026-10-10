---
type: concept
title: openclaw channel架构
tags: [openclaw, channels, 插件架构, 适配器, 消息路由]
related: [openclaw, gateway单一控制平面, local-first多端联动架构, 心跳机制heartbeat]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# openclaw channel架构

openclaw channel架构是 [[openclaw]] 接入外部通讯平台的插件化抽象层。作者将 Channels 定位为"OpenClaw 进行社交生态连接最重要的设计，将 AI 能力真正注入到用户的社交与工作动线中"。架构由 `ChannelPlugin` 统一接口、职责分离的适配器矩阵、五阶段插件生命周期与八步消息流转组成，原生支持 WhatsApp、Telegram、Slack、Discord、Google Chat、Signal、iMessage、Microsoft Teams、Matrix、Zalo 十个通讯平台。来源：[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]] 3.5 节。

## ChannelPlugin 接口

```
ChannelPlugin 接口
  id: ChannelId
  meta: ChannelMeta (label, docsPath, aliases)
  capabilities: ChannelCapabilities (chatTypes, polls, threads)
适配器（图标注"12个独立适配器"，表格实列 14 项）
可选能力: onboarding, auth, heartbeat, agentTools
```

## 适配器职责矩阵（核心 7 + 功能 7）

| 适配器 | 类别 | 职责与方法 |
|---|---|---|
| config | 核心 | 账户配置: listAccountIds, resolveAccount, isEnabled, isConfigured, setAccountEnabled |
| setup | 核心 | 账户设置: resolveAccountId, applyAccountConfig, validateAccountConfig |
| outbound | 核心 | 消息发送: sendText, sendMedia, sendPoll, editText, deleteMessage, chunker |
| status | 核心 | 状态探测: probeAccount, auditAccount, formatStatusSnapshot |
| gateway | 核心 | 生命周期: startAccount, stopAccount, getRunningAccountIds, createInboundHandler |
| security | 核心 | 安全策略: resolveDmPolicy, collectWarnings |
| pairing | 核心 | 配对管理: resolvePairing, validatePairing |
| groups | 功能 | 群组: resolveRequireMention, resolveToolPolicy |
| threading | 功能 | 线程: resolveReplyToMode, buildToolContext |
| mentions | 功能 | 提及: resolveMentions, formatMention |
| directory | 功能 | 目录: resolveUserDirectory, resolveGroupDirectory |
| resolver | 功能 | 路由: resolveAgentRoute (自定义路由逻辑) |
| actions | 功能 | 动作: resolveMessageActions, handleAction |
| messaging | 功能 | 消息扩展: resolveMessageMeta, formatMessage |

**计数矛盾（记录备查）**：3.5.1 架构图标注"12 个独立适配器"，`types.adapters.ts` 代码注释亦写"12 个适配器接口"，但本矩阵实列 14 行。差异待官方源码核证。

## 插件生命周期五阶段

1. **注册**：`registerChannel()` → PluginRegistry
2. **初始化**：轻量加载 `getChannelDock()`（元数据）/ 完整加载 `getChannelPlugin()`
3. **配置**：SetupAdapter（`resolveAccountId()`/`applyAccountConfig()`）+ ConfigAdapter（`listAccountIds()`/`resolveAccount()`）
4. **运行**：Gateway 启动 `startAccount()`（可选）→ inbound handlers → `resolveAgentRoute()` → OutboundAdapter.sendText/`sendMedia()`
5. **监控**：StatusAdapter（`probeAccount()`/`auditAccount()`）+ HeartbeatAdapter（`checkReady()`）

## 消息流转八步

**入站**：Webhook/Gateway Event → Channel Monitor（去重 & 预处理 → Allowlist 验证）→ resolveAgent Route → Session 管理（持久化会话元数据）→ Agent AI Engine

**出站**：Outbound Deliver → OutboundAdapter 加载 → 消息分块（chunker）→ Channel Outbound Adapter（Telegram: bot.ts / Discord: send.ts / Slack: send.ts / WhatsApp: web）

## binding 路由优先级（八级，从高到低）

1. `binding.peer`（精确用户/群组）
2. `binding.peer.parent`（线程继承）
3. `binding.guild + roles`
4. `binding.guild`
5. `binding.team`
6. `binding.account`
7. `binding.channel`
8. default agent

**Session Key 六段格式**：`{agentId}:{mainKey}:{channel}:{accountId}:{peerKind}:{peerId}`

## 关键设计要点五条

1. **分层抽象**：Application → Channel Abstraction → Implementation → Plugin Registry
2. **适配器模式**：职责分离的独立适配器
3. **性能优化**：Dock 轻量加载 / 延迟加载 / 路由缓存 / Update 去重
4. **扩展性**：实现 `ChannelPlugin` 接口并注册即可接入新 Channel
5. **安全隔离**：每 channel 独立 security/pairing/allowlist

## 源码目录结构（原文保真）

```
src/
├── channels/                    # Channel核心抽象
│   ├── plugins/                 # 插件系统
│   │   ├── types.plugin.ts      # ChannelPlugin接口定义
│   │   ├── types.adapters.ts    # 12个适配器接口
│   │   ├── types.core.ts        # Capabilities, Meta等
│   │   └── registry-loader.ts   # 加载器工厂
│   ├── dock.ts                  # 轻量级Dock (共享代码路径)
│   ├── registry.ts              # Channel ID规范化
│   ├── allow-from.ts            # Allowlist匹配
│   ├── channel-config.ts        # 配置匹配
│   └── session.ts               # 会话状态管理
├── routing/                     # 路由系统
│   ├── resolve-route.ts         # 路由解析 (核心: 291-443行)
│   ├── bindings.ts              # Agent绑定管理
│   └── session-key.ts           # Session Key构建
├── telegram/bot.ts              # Bot创建, Update去重
├── discord/                     # monitor.ts(Gateway连接)/send.ts(Components V2)/ui.ts(UI容器)
├── slack/  ├── signal/  ├── imessage/
├── web/                         # WhatsApp Web实现
│   └── whatsapp-heartbeat.ts    # 心跳检测
├── infra/outbound/deliver.ts    # 出站消息基础设施/消息发送流程
└── plugins/registry.ts          # PluginRegistry实现

extensions/                      # 扩展插件
├── msteams/src/channel.ts  ├── matrix/src/channel.ts
├── zalo/  └── voice-call/
```

## 关联

- [[local-first多端联动架构]]：Channels 是多端联动在通讯平台维度的落地
- [[心跳机制heartbeat]]：HeartbeatAdapter（`checkReady()`）与 WhatsApp Web 心跳检测（`whatsapp-heartbeat.ts`）是心跳机制在通道层的体现
- [[gateway单一控制平面]]：Channel 生命周期第 4 阶段经 Gateway 启动与路由
- [[任务隔离]]：`session.ts`/`session-key.ts` 的六段会话键支持频道/账户/同伴级会话隔离
