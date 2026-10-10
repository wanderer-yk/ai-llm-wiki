---
type: concept
title: Agent 裸奔四问题
tags: [harness-engineering, agent架构, 可靠性]
related: [harness四强制约束, harness与workflow主导权之辨, harness-engineering, prompt-context-harness三阶段, 验证门禁化]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Agent 裸奔四问题

Agent 裸奔四问题指没有 Harness 约束时（"裸奔"状态）Agent 完成长程任务必然遭遇的四大失败模式，是论证 Harness Engineering 存在必要性的核心论据：

1. **过早终止**：写完代码即认为任务完成，不主动验证后续环节。
2. **缺乏反思**：没有自我验证机制，无法发现自身产出的缺陷。
3. **死循环陷阱**：在同一逻辑死角无限重试，消耗资源且无法自拔。
4. **高风险场景失控**：删除文件、调用外部 API 等高危动作没有审批与熔断。

这四个问题共同指向同一结论：通用基座模型的能力不等于可靠性，需要外部运行环境提供约束、引导、检验与评估（Harness 定义）。对应的解决路径为 [[harness四强制约束]]（分步执行/强制测试/闭环修复/最终验收）与 [[harness与workflow主导权之辨]] 中的软约束路线；"高风险场景失控"一条与 vivo 的 [[验证门禁化]]（硬性阻断规则）直接同构。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
