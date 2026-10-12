---
type: concept
title: LLM输出格式异构性
tags: [llm, 结构化输出, 异常处理]
related: [结构化输出, agent系统模块六要素, deepseek]
created: 2026-10-12
updated: 2026-10-12
sources: ["[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-上.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么-下.html", "[202603240830]业务逻辑的坍塌当应用层只剩下胶水代码在AIAgent时代我们该构建什么.html"]
---
# LLM 输出格式异构性

不同模型的输出格式存在异构性，需统一提取适配（`extract_json_from_response`）：纯 JSON / ```json 代码块包裹 / DeepSeek-R1 `<think>` 思维链前缀。这是 [[韩春]] Demo 实测发现的问题，属于 [[agent系统模块六要素]] 中"输出解析"要素的具体化。

## 输出后三层降级验证（作者"段子"式总结，实际可用）

1. `json.loads` 直接解析；
2. 失败则让 LLM 修复自己的输出；
3. 再失败则正则强行提取（`extract_json_with_regex`），最后回落默认错误值。

关联：与 [[结构化输出]]（基于 Function Calling 的结构化输出机制）互补——前者是协议级解法，本文是解析层兜底解法；DeepSeek-R1 的 `<think>` 前缀参见 [[deepseek]]。