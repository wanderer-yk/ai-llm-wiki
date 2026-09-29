---
type: entity
title: Galileo
tags: [腾讯, 日志查询, 监控, 性能分析, mcp-server]
related: [claude-code, mcp, seanguo]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# Galileo

腾讯内部日志查询/监控/性能分析平台，通过 Galileo MCP 接入 [[agentic-engineering]] 工具链。

## 核心能力

- **Log Query API**：支持按 target、level 等多维度条件查询日志
- **Profile 分析**：辅助定位 OOM 等性能问题
- **MCP `ask_question` 接口**：作为降级查询通道

## 在[[十一阶段后台开发流程]]中的角色

阶段⑦（日志排查与调试）通过 `galileo-log-query` Skill 调用，内置 Python 脚本调用 Galileo Log Query API。查不到结果时自动降级到 Galileo MCP 的 `ask_question` 接口。

### 实战案例

- 成功定位 `redeem_reward.go:48` 的 cfg nil 问题并给出修复建议
- 通过 Galileo profile 辅助快速定位 OOM 问题

### 防上下文爆炸

`galileo-log-query` Skill 内置防上下文爆炸机制：默认 limit 50、建议先用 `level:error` 缩小范围，避免日志结果撑爆 Agent 上下文窗口。

## 关联来源

- [[sources/[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程|[202604171736]从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程]]