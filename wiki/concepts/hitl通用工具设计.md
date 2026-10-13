---
type: concept
title: HITL 通用工具设计
tags: [hitl, redis, 工具化, 人机协同, react-agent]
related: [human-in-the-loop, agent设计模式对跖点, react-agent, spring-ai-alibaba]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601080830]从HITLHumanInTheLoop实践出发看Agent与设计模式的对跖点.html"]
---
# HITL 通用工具设计

**HITL 通用工具设计**是奕寰提出的将 HITL（Human In The Loop）从针对单一场景的演示工具升级为可复用通用工具的工程化方案，核心是通过 **Redis 轮询挂起机制**实现对话挂起与恢复，使用户补全在单次对话内完成。它回答的是 [[human-in-the-loop]] 从理念到落地的基础设施问题：用户隔离、对话挂起与重连、超时未输入处理、以及该工具对 Agent 整体规划的影响。

## Redis 轮询挂起执行流程（7 步）

1. HITL 工具执行逻辑中，循环调用 Redis，查询 key 为 AnswerId 的数据是否有内容，没有内容时继续循环等待；
2. HITL 工具返回给 UI 界面当前对话 id 及用户需补充文案的描述（通过 url 传入 answerId 及表单参数）；
3. 用户在 UI 界面中输入；
4. UI 页面调用后端提交接口，后端接口将数据写入 Redis；
5. HITL 工具中的循环此时查询到 Redis 中的数据；
6. HITL 工具执行结束，将 Redis 中的内容作为工具的执行结果；
7. LLM 继续推理，执行下一步动作。

## 配套设计要点

- **超时判断**：不能长时间空等待，需在循环中加等待时长判断（代码为截图，文本不可恢复）。
- **用户隔离与挂起重连**：通过对话 id（AnswerId）隔离不同用户的补全上下文，支持挂起后重连。
- **规划类系统提示词**：在基础 ReactAgent 提示词中加入该工具对整体规划的影响，避免 HITL 工具的插入破坏 Agent 的任务规划（文案为截图，文本不可恢复）。
- **实现自白**：作者用 `if (toolName.equals("HITL"))` 判断进入 HITL 工具特殊处理，并直言“本质上这种设计就是一种拦截手段”——这是 [[agent设计模式对跖点]] 中"HITL ↔ Hook/拦截/切面”映射的实现注脚。
- **业界印证**：[[spring-ai-alibaba]] 的设计同样利用 Hook 钩子手段提供 HITL 能力（截图佐证，API 细节不可恢复）。

## 与相邻概念的边界

- [[human-in-the-loop]] 收录 HITL 的概念本质（挂起等待保持单次对话连续性、伪人机协同批判、工具出参=用户补全内容），本页聚焦通用工程实现。
- 与 [[上下文压缩策略]] 的关联：多轮对话式追问依赖记忆压缩，可能造成语义失真；挂起等待方案规避了重新理解提示词与前置处理的冗余。