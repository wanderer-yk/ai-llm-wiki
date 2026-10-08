---
type: concept
title: Skill 脚本自动化原则
tags: [脚本, 确定性, 概率性, agent-skill]
related: [skill渐进式披露, skill改进四原则, 自动化决策层级, anthropic, agent-skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202603091800]打造高效易用的AgentSkill.html"]
---
# Skill 脚本自动化原则

**"凡是可以用代码确定性完成的事情，就不要让模型用自然语言'理解'着去做。"** 模型理解有概率性、代码执行是确定性的——Skill 编写中应把确定性逻辑封装为 `scripts/` 下的脚本，SKILL.md 只负责告知何时调用哪个脚本、传什么参数。

## 官方实践证据

Anthropic 官方 PDF、DOCX、PPTX 等文档生成 Skill 均采用此模式：文档生成逻辑封装在 Python 脚本中，SKILL.md 仅做调用编排——官方仓库被来源称为"渐进式披露和脚本自动化的最佳实践"学习范本（见 [[anthropic]]）。

## 与改进四原则的衔接

重复劳动是脚本化的提炼信号：多个测试用例独立编写了类似辅助脚本/预处理时，应上收至 `scripts/` 复用（见 [[skill改进四原则]] 第四原则）。

## 跨来源印证

"确定性任务交给确定性手段"与 zhiyuanfu 的 [[自动化决策层级]]（五层决策金字塔，优先使用确定性最低成本层级）在决策逻辑上一致：能代码化的不进模型，能流程化的不靠悟性。
