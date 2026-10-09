---
type: entity
title: code-wiki
tags: ["code-wiki", "CLI", "Agent工具", "代码知识图谱", "UModel", "治理"]
related: ["umodel", "vibeops-agents", "agent交互层cli+skill", "意图化子命令设计", "token预算优化输出格式", "三范式量化评测基准", "验证门禁化", "knowledge-wiki", "mcp", "skill-command-mcp三层架构", "两步查询模式", "渐进式工具加载", "deepwiki", "augment-code"]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# code-wiki

**code-wiki** 是《从可观测到可理解》一文中提出的 Agent 查询侧 CLI 工具，作为 [[umodel|UModel]] 代码知识图谱的 Agent 交互入口，也是文中确认的 Agent 接入形态——**CLI + 场景化 Skill，而非 MCP**。它与有赞 [[knowledge-wiki]] 的"面向 AI 的项目知识层"理念形成命名与定位上的双重呼应。

> [!note] 归属状态
> 来源文章未说明 code-wiki 的项目归属（是否独立/开源）；截至收录时其开源归属与项目独立性未证实，与写入侧 `starops` CLI 的关系也未说明。

## 命令树（原文 verbatim）

```bash
code-wiki query <子命令>     # 图谱查询
  ├── search <keyword>       # 实体搜索
  ├── context <name>         # 符号完整上下文
  ├── impact <path>          # 变更影响分析
  ├── callers / callees      # 调用链
  ├── deps / rdeps           # 依赖 / 反向依赖
code-wiki check <子命令>     # 治理检查
  ├── arch                   # 架构违规扫描
  └── hotspots               # 耦合热点
code-wiki ingest             # 构建/更新图谱
code-wiki status             # 健康检查
```

子命令按 Agent 意图（impact/deps/callers）而非底层技术组织，屏蔽底层 graph-match 与 SLS SQL 的差异，详见 [[意图化子命令设计]]。

## 输出格式

- 默认 `--format brief`：为 Agent token 预算优化，单次 `query context` < 500 tokens；完整数据走 `--format json`（详见 [[token预算优化输出格式]]）。

brief 格式输出示例（< 500 tokens）：

```text
$ code-wiki query context pkg/a2a
Module: pkg/a2a
  LOC: 1,247 | Language: Go | Component: a2a-protocol
  Summary: A2A protocol implementation for agent-to-agent communication
Types (17): TaskStore(struct), A2AServer(struct), AgentCard(struct), .
Functions (52): HandleA2ARequest[entry], StartA2AServer[entry], .
Reverse dependencies (9): pkg/api/handler, pkg/server, cmd/vibeops-agents, .
Component crossings: → api, → scheduler
```

## 场景化 Skill 与推理路径

- 配套场景化 Skill 按三场景组织（RCA 排障 / 日常开发 / 架构治理），使 Agent 免学 SPL 语法（详见 [[agent交互层cli+skill]]）。
- 命令组合天然匹配 Agent 渐进式推理路径：search → context → impact，与 [[渐进式工具加载]] 的工具组合策略一致。

## CI 集成用法（文中设计）

```bash
code-wiki ingest --incremental        # 增量更新图谱
code-wiki check arch                  # 架构违规检查
code-wiki query impact <changed_files>  # 变更影响分析
```

这三条 CI 门禁命令是 [[验证门禁化]]（vivo 丁俊杰）主张在代码域的直接实例。

## 实证案例

三个实战案例——案例一（影响评估）、案例二（RCA 四步定位）、案例三（架构治理）——均通过 code-wiki CLI 完成，对象为 [[vibeops-agents]] 项目。案例输出为作者 VibeCoding 演示 Demo，完整命令与输出样例见 [[sources/[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱]]。

## 与相关系统的关系

- 与有赞 [[knowledge-wiki]]（`.wiki/` 面向 AI 的项目知识层）形成命名与理念呼应，但技术路线不同：前者是 UModel 图谱查询 CLI，后者是 Git submodule 分布式 Wiki。
- 与 [[deepwiki|DeepWiki]]（MCP Server 三工具）、[[augment-code|Augment Code]]（MCP 开放）构成 Agent 查询侧接入形态的三方对照。

## 开放问题

- 是否开源；Skill 文件具体内容待确认。
- 与 [[mcp]] 路线及 [[skill-command-mcp三层架构]]（seanguo）的接入形态分歧待对比裁决。