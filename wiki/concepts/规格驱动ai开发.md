---
type: concept
title: 规格驱动 AI 开发（SDD）
tags: [sdd, 规格驱动开发, ai编程, 方法论]
related: [specflow, openspec, github-spec-kit, bmad-method, ai编程幻觉, blocker-gate]
created: 2026-06-12
updated: 2026-10-08
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html", "[202603111830]AI编程能力边界探索基于ClaudeCode的SpecCoding项目实战得物技术.html"]
---
# 规格驱动 AI 开发（SDD）

**Spec-Driven Development（规格驱动开发）** 是一种以规格文档为"契约"驱动 AI 进行代码生成的开发范式。其核心理念是将模糊的需求拆解为机器可理解的精确规格，再基于规格驱动 AI 编码。

## 核心主张

- **AI 不缺代码实现能力，缺的是精准的"指令规格"**
- 当设计与逻辑契约足够清晰时，编码成为可放心交给 AI 的"廉价体力活"
- AI 辅助编码的目标应是"准"而非"快"

## 解决的问题

SDD 针对的是 AI 辅助编程中的两大核心痛点：

1. **上下文断层**：多轮对话中 AI 丢失上下文，导致前后矛盾
2. **需求共识缺失**：人与 AI 之间对需求理解不一致

## 业界方案

天玑前端团队调研了三种 SDD 方案，各自侧重不同维度：

| 方案 | 定位 | 核心理念 |
|------|------|----------|
| [[openspec\|OpenSpec]] | 轻量级、面向变更 | 原子化变更、Proposal/Apply 闭环 |
| [[github-spec-kit\|GitHub Spec Kit]] | 工业级、标准化协作 | Constitution 宪章、门控（Gating） |
| [[bmad-method\|BMAD-METHOD]] | 全能型、多代理协作 | 角色思维隔离、专家团模拟 |

## Specflow 的融合方案

[[specflow|Specflow]] 融合三者之长：OpenSpec 的轻量性 + Spec Kit 的严谨门控 + BMAD 的多角色思维，形成 Specify→Plan→Implement→Archive 四阶段流水线，深度适配 Cursor IDE。

## 研发范式前移

SDD 的核心价值是**研发范式前移**——将问题解决的关键环节从编码调试期前移至需求设计期，通过确定性流程对抗业务复杂性。"流程的确定性是对抗业务复杂性的唯一手段"。

## 别名与采用链（2026-10 定稿）

- **Spec Coding** 为本概念正式别名：得物阳凯《AI编程能力边界探索》中同义使用（案例标题直接写"SDD"，定义同为「在写代码之前，先写规格文档」，同用 [[openspec]] 工具流）——不另立页面，证据见 [[sources/[202603111830]AI编程能力边界探索基于ClaudeCode的SpecCoding项目实战得物技术|得物 Spec Coding 实战]]。
- **OpenSpec 采用链（第三团队实锤）**：爱奇艺天玑（调研借鉴）→ 腾讯 CodeBuddy（直接采用）→ **得物**（Claude Code + opsx 指令集落地 Spec Coding 项目实战）。
