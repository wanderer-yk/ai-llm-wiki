---
type: concept
title: Harness 四强制约束
tags: [harness-engineering, 软件开发, 质量门控]
related: [agent裸奔四问题, harness与workflow主导权之辨, 验证门禁化, 全生命周期hook机制, harness-engineering]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Harness 四强制约束

Harness 四强制约束是 Harness Engineering 在软件开发场景给出的四条硬性流程约束，用以对治 [[agent裸奔四问题]]：

1. **分步执行**：限制 Agent 每次只做一个模块，禁止大而全的一次性生成。
2. **强制测试**：代码完成后必须经过测试，不允许"写完即止"。
3. **闭环修复**：测试失败自动进入修复循环，直至通过。
4. **最终验收**：交付前进行自我反思与完整性检查。

作者概括其代价与收益："带着镣铐跳舞"——以模型与系统运行复杂度的增加，换取**确定性、健壮性、成功率**三项回报。

OpenClaw 中的实现示例是"强制测试器"：经 Hook（见 [[全生命周期hook机制]]）配置，代码生成后自动触发语法检查/单元测试，Bug 日志立即反馈要求修复，直到测试通过才允许交付——将"写完即止"升级为"写完必测"。该机制与 vivo 丁俊杰的 [[验证门禁化]]（未通过验证则硬性阻断）构成跨团队同构；流程哲学层面与 [[harness与workflow主导权之辨]] 的"软约束保留自主性"互补：四约束划定硬边界，边界内保留 Planning 与 Looping 自由。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
