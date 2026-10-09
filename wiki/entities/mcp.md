---
type: entity
title: MCP (Model Context Protocol)
tags: [协议, 工具接入, 标准化, agent]
related: [responses-api, a2a, 三大武器库, agent三层商用架构]
created: 2026-06-12
updated: 2026-10-09
sources: ["[202605071734]十年老技术开发的AIAgent探索之路.html", "[202601261830]AI编程实践从ClaudeCode实践到团队协作的优化思考得物技术.html", "[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# MCP (Model Context Protocol)

Agent 以标准方式接入工具、资源和外部系统的协议。MCP 工具定义采用 JSON 格式，包含 input_schema、permissions、rate_limit 等标准化字段。

在 [[zhiyuanfu]] 的行业分析中，MCP 代表工具接入标准化趋势。与 [[三大武器库]]（腾讯 binxiong）中的 MCP 连接层直接对应。作者核心判断：协议层是长期资产，框架是短期工具；选技术栈优先看是否兼容 MCP/Responses API。

## 来自得物技术篇的证据（2026-01）

- **得物飞书 MCP 三场景（2026-01）**：feishu_create_doc（文档创建）/ feishu_get_doc_content（文档读取）/ feishu_append_bitable_data（多维表追加）——子代理系统产出的结构化落点。服务器为官方还是自建源文未说明，待查。

## 源码级机制证据（百度 Cheer，2026-04）

- **双位置注入**：MCP 工具经 `tools[]` 注册（`toolToAPISchema` 转换，命名 `mcp__<serverName>__<toolName>`）+ server `instructions` 注入 system 动态区（`getMcpInstructions`）；执行走真实 JSON-RPC 调用链 （证据等级：v2.1.88 泄漏源码、版本特定、非官方口径，引用链未独立核实）。
- **内置工具同构论**：Claude Code 部分内置能力（如 Bash）与 MCP 工具在协议层同构（Q2 源码验证），详见 [[mcp内置工具同构论]]、[[comparisons/mcp与bash对比]]。
- **instructions 落地缺位**：协议字段在 Claude Code 实现中注入但多数 server 未有效利用；「Bash 优先」选型条件见 [[mcp与bash对比]]——简单一次性操作 Bash 更省，需状态/复用/权限隔离才上 MCP。
