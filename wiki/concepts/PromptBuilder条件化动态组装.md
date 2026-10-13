---
type: concept
title: "Prompt Builder 条件化动态组装"
tags: [prompt-builder, 动态prompt, 上下文组装, AssistantAgent, 非侵入式]
related: [AssistantAgent, 评估图, 动态prompt生成机制, 结构化上下文组装, 知识优先级机制]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202601150830]阿里AssistantAgent开源助力开发者快速构建答疑诊断智能助手.html"]
---
# Prompt Builder 条件化动态组装

Prompt Builder 是 [[AssistantAgent]] 的 Prompt 动态组装模块：根据[[评估图|评估结果]]和运行时上下文**条件化**决定注入哪些 Prompt 片段，再拼接为最终动态 Prompt 送入模型；采用**非侵入式**设计，在模型调用层拦截，不改写业务代码。

## 四项核心能力

1. 多个 PromptBuilder 按**优先级顺序**执行。
2. 每个 Builder 根据[[评估图|评估结果]]决定是否贡献、贡献什么。
3. 支持自定义 Builder 定制业务 Prompt 逻辑。
4. 非侵入式：在模型调用层拦截。

## 条件匹配规则

| 评估条件 | 注入片段 |
|---------|---------|
| 模糊=是 | [澄清引导 Prompt] |
| 模糊=否 | [直接执行 Prompt] |
| 经验=有 | [历史经验参考] |
| 工具=有 | [工具使用说明] |
| 知识=有 | [相关知识片段] |
| 组合：模糊+无工具+无知识 | [追问用户 Prompt] |
| 组合：清晰+有工具+有经验 | [快速执行 Prompt] |

最终动态 Prompt = [系统提示] + [澄清引导] + [历史经验] + [工具说明] + [用户问题] → 模型。

组合路由体现了"评估结果 → 执行策略"的映射：输入模糊且无资源可用时退化为追问用户，输入清晰且工具与经验齐备时走快速执行——确定性分支前置到 Prompt 组装层完成。

## 与既有概念的关联

- 与 [[动态prompt生成机制]]（阿里商旅 aligo）高度同构：两者都以上游结构化结果驱动 Prompt 生成，而非静态模板。
- 与 [[结构化上下文组装]]（有赞 XML 标签分层 + 优先级）互补：Assistant Agent 的分层来自 Builder 优先级与评估条件，有赞的分层来自 XML 标签与 Prompt 内优先级声明。
- 待披露：自定义 PromptBuilder 的编程接口与条件配置格式（原文仅存"对比传统方案"图片，文字未展开）。
