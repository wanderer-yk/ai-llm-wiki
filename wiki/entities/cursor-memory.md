---
type: entity
title: Cursor Memory / Claude Memory
tags: [tool, ai-coding, local-memory, cursor, claude]
related: [cursor, claude-code, 知识基座-天猫]
created: 2026-07-21
updated: 2026-07-21
sources: ["[202603231539]知识基座让AI越用越懂业务的团队经验实践天猫AICoding实践系列.html"]
---
# Cursor Memory / Claude Memory

Cursor Memory 和 Claude Memory 分别是 [[cursor]] 编辑器和 [[claude-code]] 的本地记忆功能，属于个人级记忆机制。

## 四维局限

天猫团队对 Cursor Memory / Claude Memory 与 [[知识基座-天猫|知识基座方案]] 进行了四维度对比：

| 维度 | Cursor Memory / Claude Memory | 知识基座方案 |
|------|-------------------------------|-------------|
| 存储位置 | 本地存储，跟着个人设备走 | 云端存储 |
| 作用范围 | 单仓库单用户 | 跨仓库跨用户，按业务域隔离 |
| 沉淀机制 | 全量记忆压缩 | 信号驱动提取 |
| 知识质量 | 混杂无价值对话 | 聚焦踩坑经验 |

## 核心局限

Cursor 等工具本质是"个人助手"，记住的是个人偏好而非团队知识。其记忆语义为"我上次说过什么"而非"团队积累了哪些经验"。超级个体的经验被锁定在个人本地配置中，无法惠及团队。

这一局限直接驱动了天猫团队构建 [[知识基座-天猫|知识基座系统]]，实现从"个人记忆"到"团队知识共享 + 智能沉淀"的升级，对应 [[企业级vs个人工具本质区别论]] 的核心论点。