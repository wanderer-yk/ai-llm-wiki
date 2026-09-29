---
type: concept
title: SOTA 模型专属优化
tags: [设计哲学, 模型策略, qoder, token效率]
related: [ai-coding第一性原理, 代码作为中间产物, qoder, token效率法则]
created: 2026-06-22
updated: 2026-06-22
sources: ["[202601242318]QoderQuest10把执行交给AI把选择留给人类.html"]
---
# SOTA 模型专属优化

"SOTA 模型专属优化"是 [[qoder]] Quest 模式的核心设计策略，指不做向下兼容，仅为当前最强（SOTA, State-Of-The-Art）大模型进行优化，以最快接近 SOTA Coding Agent 的能力。

## 设计逻辑

### 为何舍弃兼容

- **性能最大化** — SOTA 模型的能力上限决定了 Agent 的能力上限
- **架构简洁** — 无需处理不同模型的 API 差异和能力参差
- **快速跟进** — 新模型发布时可最快适配，无需等待兼容性验证

### 底座架构

Quest 1.0 底座架构推测为 opus、codex、gemini 组合，均为当前最强编码模型。

### 与 Editor 模式的对比

- **Quest 模式** — 仅适配 SOTA 模型，无需切换模型，架构最快跟进 SOTA 发展
- **Editor 模式** — 需适配不同模型以渐进演进，支持更广泛的模型选择

## Token 效率法则

SOTA 专属优化策略隐含的经济学逻辑是"Token 效率法则"——不关注模型单价，转向追求 Token 效率及最终产物质量。SOTA 模型虽然单次 Token 成本更高，但通过更高的 Token 效率（更少的多轮修正、更好的首次产出质量）获得总体收益。

## 跨领域关联

- 在当前 Wiki 中，SOTA 专属优化是独特的模型策略选择。多数团队选择适配多模型：
  - 有赞 AI 客服使用 Qwen 替代 GPT-4.1 做意图识别（成本优先）
  - 美团使用"高阶模型审查低阶模型"（分层使用）
  - [[deepseek-v3]] 被有赞 Code Insight 选用（综合成本低且效果良好）
- 与 [[脚手架优于模型]] 形成有趣的张力——前者认为应优先升级模型（SOTA），后者认为应优先升级脚手架（Skill/harness）。可能的调和：在 SOTA 模型基础上构建最优脚手架