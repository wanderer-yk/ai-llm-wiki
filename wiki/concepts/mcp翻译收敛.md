---
type: concept
title: mcp翻译收敛
tags: [mcp, integration, runtime, claude-code]
related: [mcp, 动态能力面稳定内部对象, skill-command-mcp三层架构, 能力面汇总]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604150830]ClaudeCode源码拆解从启动到多Agent扩展层.html"]
---
# mcp翻译收敛

mcp翻译收敛是 Claude Code 对 MCP 的消费侧机制：MCP 接入的重点不是「连上服务器」，而是「把外部能力吸收到本地运行时模型里」。Claude Code 通过四条映射，将 MCP 原生对象翻译为内部运行时对象：

| MCP 原生对象 | Claude Code 内部对象 |
|---|---|
| MCP prompt | Command |
| MCP tool | Tool |
| MCP resource | 资源/资源工具体系 |
| （鉴权需要时） | 注入 auth tool |

翻译收敛后，MCP 能力与本地 tools、plugin commands、动态 skills 一同进入[[能力面汇总]]，服从 [[动态能力面稳定内部对象]] 原则——协议是外部的、对象是内部的。

跨源关联：与 seanguo 的 [[skill-command-mcp三层架构]]（Skill→Command→MCP 供给侧分层）互为镜像——本文是消费侧收敛视角；应回链更新 [[mcp]] 页的消费侧机制部分。
