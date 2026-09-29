---
type: entity
title: knowledge-search-experience
tags: [tool, knowledge-retrieval, ai-coding, tmall, alibaba]
related: [知识基座-天猫, search-knowledge]
created: 2026-07-21
updated: 2026-07-21
sources: ["[202603231539]知识基座让AI越用越懂业务的团队经验实践天猫AICoding实践系列.html"]
---
# knowledge-search-experience

`knowledge-search-experience` 是 [[知识基座-天猫|天猫知识基座系统]] 中 AI 自动召回团队经验的工具/命令。与 [[search-knowledge]] MCP Server 检索工具同属知识基座召回体系。

## 特色能力

`knowledge-search-experience` 不仅展示通用方案，还支持结合用户实际代码配置文件做针对性分析。例如在 TypeScript tsconfig.json 配置问题场景中，AI 通过该工具召回知识库经验后，结合用户实际的 tsconfig.json 内容进行个性化分析和解决方案推荐。

## 价值闭环案例

同学 A 运行 `tsc --noEmit` 遇到 `Cannot find type definition file for 'react-native'` 错误，经历多轮调试花费 30-60 分钟。两周后同学 B 遇到同类问题，AI 通过 `knowledge-search-experience` 自动召回经验并结合 B 的实际配置做针对性分析，B 在 1 分钟内解决。这是"A 踩坑→自动沉淀→B 受益"完整闭环的典型范例。