---
type: concept
title: OpenClaw 八层路由优先级链
tags: [openclaw, 路由, 会话管理]
related: [openclaw, openclaw会话键结构化设计, 受控子Agent机制]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603260830]深入理解OpenClaw技术架构与实现原理下.html"]
---
# OpenClaw 八层路由优先级链

OpenClaw 把消息路由到 Agent 的八层优先级（3.13 节，从高到低）：

1. **peer**（精确对等）
2. **peer.parent**（线程继承父频道绑定）
3. **guild+roles**（Discord 双约束）
4. **guild**
5. **team**（Slack）
6. **account**（非通配）
7. **channel**（`accountId: "*"` 通配）
8. **default**（`defaultAgentId`，默认 `main`）

## 配套设计

- DmScope 四级作用域：main / per-peer / per-channel-peer / per-account-channel-peer；
- 会话键 6 种格式（`agent:{agentId}:{mainKey}` 系列结构化组合，覆盖 DM/群组/线程维度）；
- identityLinks 跨平台身份合并（同一自然人的多平台账号归一）；
- 性能三缓存：绑定缓存（WeakMap ≤2000 条）+ 文件缓存（inode/dev/size/mtime）+ 模板 Promise 缓存；
- 五项设计决策：确定性路由、显式优先级、安全边界、性能优先、可扩展性。

## 跨源关联

八层显式优先级与 [[受控子Agent机制]] 的层级治理同题：路由决定「消息给谁」，任务抽象决定「谁来做」。
