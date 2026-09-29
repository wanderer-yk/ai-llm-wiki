---
type: entity
title: Kwaipilot
tags: [ai-coding, tool, agent, ide, code-generation]
related: [快手技术团队, kat-coder, codeflicker, flow, ai-coding推广三阶段, 通用工具通用效果瓶颈, cursor, claude-code]
sources: ["[202602112001]快手万人组织AI研发范式跃迁之路.html"]
created: 2026-06-22
updated: 2026-06-22
---
# Kwaipilot

Kwaipilot 是快手自研的 AI Coding 工具，2024 年建设并发布给公司内 10000+ 研发人员使用。海外版产品名为 [[codeflicker]]。

## 产品形态

Kwaipilot 覆盖多种产品形态：

- 多 IDE 插件（支持 JetBrains、Android Studio、XCode、VS Code 等主流 IDE）
- AI IDE 形态
- CLI 工具
- 智能问答引擎

底层采用统一的 Agent 架构，保证不同 IDE 插件形态下的同等服务质量。产品层多形态、能力层统一架构是其核心设计原则。

## 三代演进路径

Kwaipilot 已完成三代演进，与 [[l1-l2-l3-ai研发范式]] 路线一一对应：

1. **Code Copilot**（对应 L1）：基础代码补全和生成
2. **Code Agent**（对应 L2）：任务级 AI 协同开发
3. **Multi-Agent & Agentic Coding**（对应 L3）：多 Agent 协同的端到端交付

## 核心能力

- **内置 [[kat-coder]]**：快手自研代码大模型，定期注入快手真实代码和研发过程数据，使模型"懂快手系统"
- **业务知识库接入**：建立业务 & 研发知识库供 Kwaipilot 读取，解决通用 AI 工具不"懂"业务的问题
- **Web 开发全闭环**：包括 Figma 设计稿转代码、实时预览调试、指定元素优化
- **非编码能力**：浏览器操作（写文档/总结/调研等非编码"杂活"）
- **衍生工具**：智能 CR（CodeReview）、智能测试用例生成、智能单元测试

## 自研决策

快手经过一年期内部 AB 实验后坚定自研路线，核心判据为 Kwaipilot 的 AI 代码生成率持续优于外部产品。同时采用"用脚投票"策略——允许开发人员自由使用任何第三方 AI Coding 工具（[[cursor]]、[[claude-code]] 等），通过并行竞争验证自研产品价值。

2025 年 12 月起，出于安全原因，按代码涉密等级逐步封禁第三方 AI Coding 工具。

## 量化效果

- AI 代码生成率：从 1% 提升至 30%+（部分业务线达 40%+）
- 智能化 1.0 阶段代码生成率天花板为 24%+
- 已历经数千名研发同学反馈与打磨