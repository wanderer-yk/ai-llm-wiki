---
type: concept
title: memory-skill-nudge 三子系统自进化闭环
tags: [hermes-agent, self-improving, memory, skill, nudge-engine]
related: [hermes-agent, memory容量上限倒逼压缩, 快照冻结与前缀缓存, skill自动创建触发条件, 双计数器nudge触发, 后台审查agent, 用得越久越好用]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# memory-skill-nudge 三子系统自进化闭环

memory-skill-nudge 三子系统自进化闭环是来源文章对 [[hermes-agent]] Self-Improving 实现的总纲性概括：**Memory**（"事实小本子"，记住你是谁）、**Skill**（"操作手册"，记住怎么做事）、**Nudge Engine**（"定时反思闹钟"，保证循环不停转）三个子系统协同，构成"任务执行 → 计数触发反思 → 后台审查 → 写入 Memory/Skill → 下个会话注入"的经验积累闭环，区别于"大多数 Agent 每次会话结束后就失忆"的默认形态。文章总结章节给出官方一句话定义："Memory 记住你是谁，Skill 记住怎么做事，Nudge Engine 保证这个循环不停转。"

## 三子系统分工

- **Memory**：双文件（MEMORY.md 存环境事实/项目约定/工具怪癖，USER.md 存用户偏好/沟通风格/工作习惯），容量受限、写声明式事实（见 [[memory容量上限倒逼压缩]]、[[声明式事实记忆]]）。
- **Skill**：SKILL.md + YAML frontmatter 的操作手册，Pitfalls 由 Agent 踩坑后追加，支持自动创建与局部 patch（见 [[skill自动创建触发条件]]、[[skill局部patch修补]]）。
- **Nudge Engine**：双计数器定时触发后台审查，把经验沉淀动作从主任务中剥离（见 [[双计数器nudge触发]]、[[后台审查agent]]）。

## 闭环运转

计数器达到阈值（默认 10）→ 在用户响应发送之后 fork 后台 review agent（不占用户 attention budget）→ 审查产出写入 Memory/Skill → 由于快照冻结机制（[[快照冻结与前缀缓存]]），新经验在下个会话才注入系统提示词生效。

## 语义桥接

- 记忆、技能与触发三者的协同关系，可与本 Wiki 的 [[内外双驱记忆架构]]、[[全生命周期hook机制]]（0424 飞樰文来源）交叉对照。
- 与 [[openclaw]] 的对比为作者单方定性："靠人喂 vs 自己长"。