---
type: concept
title: Prompt 极简主义
tags: [openclaw, prompt-engineering, 极简设计]
related: [openclaw, openclaw系统提示词23模块, promptmode三级模式, context-window三段构成]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# Prompt 极简主义

Prompt 极简主义是 [[openclaw]] Prompt 措辞风格的设计立场：原文以"'质量大于数量'的极简主义"（Quality > quantity）为题专门论述——用一句 `Quality > quantity` 替代冗长的群聊行为规范，用 `Ask anything you're uncertain about` 替代大段不确定性处理指令。极简指令的价值在于为 AGENTS.md/USER.md 等业务数据腾出 Context Window 额度，提升性价比与效率。

作者的结论："优秀的 Prompt 不是写得越长越好，而是越清晰、越模块化、越节省资源越好。"OpenClaw Prompt Engineering = 结构化设计 + 动态组装（[[openclaw系统提示词23模块]]）+ 简洁主义的最佳实践。该立场与 [[promptmode三级模式]]（按场景裁剪加载范围）共同体现"token 预算是稀缺资源"的 [[context-window三段构成]] 意识。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
