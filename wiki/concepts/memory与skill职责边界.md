---
type: concept
title: memory 与 skill 职责边界
tags: [hermes-agent, memory, skill, 上下文工程]
related: [hermes-agent, 声明式事实记忆, skill自动创建触发条件, memory-skill-nudge三子系统自进化闭环]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# memory 与 skill 职责边界

memory 与 skill 职责边界是 [[hermes-agent]] 对两类持久经验资产的职责划分：**Memory 存"事实"**（用户偏好、环境细节、工具怪癖、稳定约定），**Skill 存"操作步骤"**（做成某件事的方法）。边界由工具 Schema 直接约定："If you've discovered a new way to do something, save it as a skill"（发现了新的做事方法，存为 Skill）。

## 分离的设计理由（文章设计取舍表第 2 条）

| 设计决策 | 表面效果 | 背后的考量 |
|---------|---------|-----------|
| 声明式事实 vs 操作步骤分离 | Memory 存事实，Skill 存步骤 | 两者的更新频率、触发条件、安全风险完全不同 |

## 三会话案例中的体现

在 [[三会话自进化实证案例]] 的会话 2 中，Review Agent 的三动作同时命中双通道：写入用户画像与 registry 地址（Memory，事实）、patch Skill 补 ALLOWED_HOSTS 坑（Skill，步骤）——展示了双通道同时进化的最小完整闭环。