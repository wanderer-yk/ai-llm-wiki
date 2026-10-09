---
type: concept
title: MCP instructions 落地缺位
created: 2026-10-09
updated: 2026-10-09
tags: [mcp, system-prompt]
related: [mcp, mcp内置工具同构论, system静态动态缓存分区, api请求位置决定论]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# MCP instructions 落地缺位

MCP instructions 落地缺位指：MCP 协议与 Claude Code 源码层面都支持 Server 级 `instructions` 字段——Server 可在 `initialize` 握手时返回可选 instructions，客户端经 `getMcpInstructions()` 拼接后注入 system 动态区，为该 Server 的工具集提供**整体使用手册**——但现实中**大多数 MCP Server 作者根本没写**这一字段。机制存在、生态缺位。

## description 与 instructions 的分层

- `tools[].description`：单工具说明书（来自工具的 `prompt()` 方法）；
- system 中的 instructions：Server 工具集的整体使用手册，承载全局性使用指南，非单工具说明。

## 源码佐证（`src/constants/prompts.ts`）

`getMcpInstructions` 只拼接 `type === "connected"` 且有 `instructions` 的 Server；没有任何 instructions 时返回 null——源码已为空值做好兜底，侧面印证该字段的低使用率。

## 关联

- instructions 注入位置受 prompt 缓存保护策略约束（feature gate `isMcpInstructionsDeltaEnabled()`，见 [[system静态动态缓存分区]]）；
- 对 MCP Server 开发者的启示：写好 instructions 是让 Agent 正确使用工具集的低成本高杠杆动作。
