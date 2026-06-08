---
type: concept
title: 研发范式前移（Shift Left）
tags: [研发范式, shift-left, 方法论]
related: [specflow, spec-driven-development, liu-cheng-que-ding-xing]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# 研发范式前移（Shift Left）

**研发范式前移** 是 [[tian-ji-qian-duan-tuan-dui|天玑前端团队]] 在文章总结中提出的核心理念：将质量保障与问题解决的着力点从"编码调试期"前推至"需求设计期"。

## 核心论点

1. **"准"比"快"更重要**：AI 辅助编码应追求准确性而非单纯的速度
2. **编码是廉价的体力活**：当契约（规格）足够清晰时，编码成为可交由 AI 的确定性工作
3. **核心竞争力转移**：未来开发者的核心竞争力不再是写代码的速度，而是：
   - **定义需求**的能力
   - **拆解任务**的能力
   - **驾驭规范**的能力

## 在 Specflow 中的体现

[[specflow|Specflow]] 的整个设计都是"研发范式前移"的具体落地：

- **[[blocker-gate|Blocker Gate]]**：强制在编码前完成需求澄清和技术建模
- **Specify → Plan → Implement 的阶段顺序**：确保"先想清楚再写清楚"
- **SSOT 单文档策略**：将规格作为编码的单一信息源

## 与其他 Wiki 概念的呼应

- [[ai-you-hao-yan-fa-gui-fan|AI 友好研发规范]]（美团）中的"规范固化为强制约束"体现了同样的前移思想
- [[context-engineering|上下文工程]]（马上消费）中的"保障信息准确交付"也是前移理念的一部分
- [[liu-cheng-que-ding-xing|流程确定性]] 是研发范式前移的必然结果——通过标准化流程对抗业务复杂性