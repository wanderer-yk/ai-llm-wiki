---
type: concept
title: AI 编程幻觉
tags: [ai-coding, 问题, 幻觉]
related: [cursor, specflow, spec-driven-development, vibe-coding]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# AI 编程幻觉

**AI 编程幻觉** 指 [[cursor|Cursor]] 等 AI 编程工具生成看似合理但实际错误的代码或建议的现象。这是 AI 辅助开发中的核心问题之一。

## 表现形式

在 AI Coding 场景下，"幻觉"主要表现为：
- **代码错误**：生成的代码逻辑看似正确但实际存在缺陷
- **架构偏离**：AI 偏离了项目的既定架构模式或设计规范
- **需求误解**：AI 对用户意图的理解与实际需求存在偏差

## 根因分析

[[tian-ji-qian-duan-tuan-dui|天玑前端团队]] 在实践中总结出两大根因：

1. **上下文断层**：多轮对话中关键信息丢失或被稀释，AI 缺乏完整的项目上下文
2. **需求共识缺失**：开发者与 AI 之间对需求的理解存在偏差，且随迭代不断放大

这两个问题在 [[vibe-coding|Vibe Coding]] 模式下尤为突出，因为缺乏标准化的前置约束机制。

## 解决路径

[[specflow|Specflow]] 通过 [[spec-driven-development|规格驱动开发（SDD）]] 方法论来"治愈"AI 编程幻觉：

- **[[blocker-gate|Blocker Gate]]**：强制在编码前完成需求澄清和技术建模，阻断"带着幻觉编码"
- **[[ssot-dan-wen-dang-ce-lue|SSOT 单文档策略]]**：将所有信息集中在 `plan.md` 中，消除 AI 跨文件检索时的信息丢失
- **断点 Review 机制**：每完成一个 Group 后强制展示 Diff 等待人工授权，及时发现和纠正幻觉输出