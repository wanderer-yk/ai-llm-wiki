---
type: concept
title: mcp工具封装模式
tags: [mcp, 工具封装, cursor]
related: [六步智能提示词生成法, mcp, cursor, skill-command-mcp三层架构]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202512261820]回收团队基于Cursor集成MCP的智能代码修复提示词生成实践.html"]
---
# mcp工具封装模式

通过 `@mcp.tool()` 装饰器将复杂多步逻辑封装为 AI 可调用的单一工具的设计模式。外部只需传参，内部自动完成全流程。

## 核心特征

- **单一入口**：`auto_fix_sonar_issues` 工具接受 project_name/cookie/base_dir/branch/page_size 五个参数
- **内部编排**：自动完成 Sonar API 调用 → 类型映射 → AST 解析 → 上下文提取 → 模板生成 → 示例注入的完整六步流水线
- **独立容错**：每个 issue 独立 try/except 处理，单个失败不影响其余

## Cursor 集成

通过 `~/.cursor/mcp.json` 注册 MCP 服务，每个服务包含 command（解释器路径）、args（脚本绝对路径）、env（环境变量）三个字段。在 Cursor 聊天窗口通过 `@服务名` 调用工具。修改配置后需重启 [[cursor]] 生效。

## 跨源关联

与腾讯 seanguo 的 [[skill-command-mcp三层架构]]（Skill→Command→MCP Server）理念一致：将复杂逻辑封装为可复用的工具层，降低 AI 调用门槛。转转展示了 [[mcp]] 在"Sonar 问题自动修复"场景的具体应用，是 MCP 协议在代码质量领域的工程化实践。