---
type: concept
title: MCP 内置工具同构论
created: 2026-10-09
updated: 2026-10-09
tags: [mcp, claude-code, tool-use]
related: [mcp, claude-code, api请求位置决定论, mcp-instructions落地缺位, mcp与bash对比, rules-skills-mcp选型指南]
sources: ["[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---
# MCP 内置工具同构论

MCP 内置工具同构论是本文导读三问 Q2 的定谳结论：**MCP 工具与 LLM 内置工具（Read/Edit/Bash 等）对模型没有任何区别**——两者在 `tools[]` 数组中的格式完全一致（name / description / input_schema），调用方式一样，模型无法区分；区别纯粹在 **Agent 侧的执行路由**（内置工具本地执行 vs `mcp__` 前缀转发外部 MCP Server）。这也回答了「为什么需要 MCP 协议」——从模型视角看并不需要，MCP 的价值在工程组织层（见 [[mcp与bash对比]]）。

## 双位置注入（并入）

MCP 在 API 请求中占据两个位置：

1. **`tools[]` 注册**：经 `toolToAPISchema()` 转换，命名模式 `mcp__<serverName>__<toolName>`，与内置工具注册方式完全一致：

```javascript
async function toolToAPISchema(tool, options) {
    return {
        name: tool.name, // 如 "mcp__github__create_issue"
        description: await tool.prompt(),
        input_schema: tool.inputJSONSchema
    };
}
```

2. **system 动态区 instructions**：`getMcpInstructions()` 拼接有 instructions 的 Server 使用手册，位于缓存边界标记之后；feature gate `isMcpInstructionsDeltaEnabled()` 开启时改走 attachment 以保护 prompt 缓存（见 [[system静态动态缓存分区]]、[[mcp-instructions落地缺位]]）。

## 真实 RPC 执行链（并入）

与 Skills 的提示词注入不同，MCP 执行是真实的函数调用/RPC：

```text
模型输出 tool_use: { name: "mcp__github__create_issue", input: {...} }
↓
Claude Code 识别 mcp__ 前缀，路由到对应 MCP Client
↓
MCP Client 发送 JSON-RPC 请求到 MCP Server 进程
↓
MCP Server 执行实际操作（如调用 GitHub API）
↓
tool_result.content = MCP Server 的真实输出
```

`tool_result` 装的是外部世界真实数据，而非注入的提示词。

## 配置要点

MCP 服务器定义于 `~/.claude.json`（user scope）或项目根目录 `.mcp.json`（project scope）；`initialize` 握手返回工具列表 + 可选 `instructions` 字段。
