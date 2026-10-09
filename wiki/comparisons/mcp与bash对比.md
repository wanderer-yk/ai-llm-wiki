---
type: comparison
title: MCP 与 Bash 对比
created: 2026-10-09
updated: 2026-10-09
tags: [mcp, bash, claude-code, 选型]
related: [mcp, claude-code, api请求位置决定论, mcp内置工具同构论, mcp-instructions落地缺位, rules-skills-mcp选型指南]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# MCP 与 Bash 对比

本页汇总 Cheer 基于 Claude Code v2.1.88 泄漏源码对 MCP 与 Bash 的完整对照论述。文内「祛魅论」与「不可替代三场景」是同一论述的两半（自我平衡结构，非矛盾）：前者说明 MCP 大部分场景可被 Bash 替代，后者划定 MCP 真正的不可替代边界。

## 祛魅论：很多场景下一条 Bash 就够了

- **对模型无本质差异**：MCP 调用结果与 Bash 命令结果都是 `tool_result` 文本，模型消费方式完全相同（[[mcp内置工具同构论]]）；
- **MCP 的额外成本**：Server 进程管理 + JSON-RPC 协议层 + 配置维护（`~/.claude.json` / `.mcp.json`）；
- **典型替代**：查 GitHub 用 `gh`、读数据库用 `psql`、调 API 用 `curl`——大量 MCP Server 做的事一条命令即可完成。

## 不可替代三场景与价值重定位

MCP 的价值**不在「能调用外部系统」（Bash 也能），而在「以更安全、更可靠的方式调用外部系统」**：

1. **持久化连接和状态管理**：数据库连接池、WebSocket、共享认证 session 等需要常驻进程的场景——Bash 每次调用都是新进程，无法维持状态；
2. **复杂操作原子封装**：5 步 Bash 管道封装为 1 次 MCP 调用，降低出错面与模型推理负担；
3. **权限隔离和安全约束**：限制模型只能执行 Server 预定义的操作，而非给它万能 Bash。

## 选型结论

简单 CLI 操作直接用 Bash，别折腾 MCP；需要上述三种能力时才引入 MCP（完整清单见 [[rules-skills-mcp选型指南]]）。引入 MCP 后应注意写好 Server 级 `instructions`——现实中大多数 Server 作者没写（[[mcp-instructions落地缺位]]）。
