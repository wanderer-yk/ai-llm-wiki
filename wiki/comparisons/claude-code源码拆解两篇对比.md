---
type: comparison
title: claude-code源码拆解两篇对比
tags: [claude-code, 源码分析, 跨来源对比, 对比]
related: [claude-code, queryloop状态机, autocompact水位线机制, fork-sub-agent机制, system-prompt动态组装机制]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html", "[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# claude-code 源码拆解两篇对比

千问AI平台两篇独立反向工程（无岳 04-15 vs 飞樰 04-20）对同一对象的两套拆解——互证度与互补面如下（review-3435329b 立项①；②③④跨源候选登记不立）。

## 互证（两篇独立得出一致结论）

| 机制 | 无岳篇 | 飞樰篇 |
|---|---|---|
| 压缩分档 | snip/microcompact/collapse/autocompact 四档 | [[autocompact水位线机制]]+[[microcompact工具白名单]]（双机制细名互证） |
| fork 子 Agent | Task 统一抽象下的执行体 | [[fork-sub-agent机制]]（后台审查 attention budget 细节） |
| CLAUDE.md 分层 | （未展开） | [[claude-md四路径分层]] |

## 互补（各自独有）

- **无岳独有**：权限四层链路（规则→判定→交互→隔离）、统一任务抽象 Task 接口、扩展层收敛命题（[[mcp翻译收敛]]）、三主干链路总架构、五条带走结论；
- **飞樰独有**：system-prompt 动态组装与优先级链、system-reminder 注入、cacheScope 分级缓存、memdir 结构化记忆、verification-agent 五大哲学、九段式摘要模板。

## 合并视图

两篇合并后 Claude Code 反向工程覆盖度显著提升：无岳=「运行时骨架」（启动→REPL→QueryLoop→ToolRuntime→Permission→Task→扩展层七层），飞樰=「上下文内容工艺」（Prompt/Context/Harness 三层次）。前者回答"怎么跑得稳"，后者回答"上下文怎么放得对"——与 [[harness工程三部曲演进论]] 的命题统一（放什么）互补于工程骨架（怎么跑）。
