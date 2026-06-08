---
type: concept
title: 流程确定性
tags: [流程, 确定性, 方法论]
related: [yan-fa-fan-shi-qian-yi, specflow, ai-you-hao-yan-fa-gui-fan]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# 流程确定性

**流程确定性** 是通过标准化流程对抗业务复杂性的核心理念。[[tian-ji-qian-duan-tuan-dui|天玑前端团队]] 在文章总结中明确提出：**流程确定性是对抗业务复杂性的唯一手段**。

## 核心思想

在 AI Coding 时代，业务复杂性与 AI 生成的不确定性叠加，使得开发过程更加不可控。流程确定性通过：

- **标准化阶段**：将开发过程拆解为固定阶段（Specify→Plan→Implement→Archive）
- **硬性门控**：通过 [[blocker-gate|Blocker Gate]] 强制遵守阶段纪律
- **结构化交互**：通过 `[User]` 区域和 `[Block]/[?]` 分级实现确定性的人机交互
- **可追溯存证**：通过 Log 存证和归档机制确保过程可回溯

## 与 Wiki 其他主题的呼应

- [[ai-you-hao-yan-fa-gui-fan|AI 友好研发规范]]（美团）：规范固化为 [[always-ji-bie-ai-rule|always 级别 AI Rule]]，体现了同样的"确定性约束"思想
- [[yan-fa-fan-shi-qian-yi|研发范式前移]]：流程确定性是范式前移的必然结果
- [[an-quan-tie-lv|安全铁律]]（马上消费）：写操作执行前强制授权，是流程确定性的另一种体现