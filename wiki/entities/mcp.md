---
type: entity
title: MCP (Model Context Protocol)
tags: [协议, 工具接入, 标准化, agent]
related: [responses-api, a2a, 三大武器库, agent三层商用架构]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html"]
---
# MCP (Model Context Protocol)

Agent 以标准方式接入工具、资源和外部系统的协议。MCP 工具定义采用 JSON 格式，包含 input_schema、permissions、rate_limit 等标准化字段。

在 [[zhiyuanfu]] 的行业分析中，MCP 代表工具接入标准化趋势。与 [[三大武器库]]（腾讯 binxiong）中的 MCP 连接层直接对应。作者核心判断：协议层是长期资产，框架是短期工具；选技术栈优先看是否兼容 MCP/Responses API。