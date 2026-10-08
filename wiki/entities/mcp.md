---
type: entity
title: MCP (Model Context Protocol)
tags: [协议, 工具接入, 标准化, agent]
related: [responses-api, a2a, 三大武器库, agent三层商用架构]
created: 2026-06-12
updated: 2026-10-08
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html", "[202601261830]AI编程实践从ClaudeCode实践到团队协作的优化思考得物技术.html"]
---
# MCP (Model Context Protocol)

Agent 以标准方式接入工具、资源和外部系统的协议。MCP 工具定义采用 JSON 格式，包含 input_schema、permissions、rate_limit 等标准化字段。

在 [[zhiyuanfu]] 的行业分析中，MCP 代表工具接入标准化趋势。与 [[三大武器库]]（腾讯 binxiong）中的 MCP 连接层直接对应。作者核心判断：协议层是长期资产，框架是短期工具；选技术栈优先看是否兼容 MCP/Responses API。

## 来自得物技术篇的证据（2026-01）

- **得物飞书 MCP 三场景（2026-01）**：feishu_create_doc（文档创建）/ feishu_get_doc_content（文档读取）/ feishu_append_bitable_data（多维表追加）——子代理系统产出的结构化落点。服务器为官方还是自建源文未说明，待查。
