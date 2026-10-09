---
type: concept
title: token 预算优化输出格式
tags: [token优化, 输出格式, CLI设计, agent上下文]
related: [code-wiki, 两步查询模式, 分块token预算推导, skill列表token预算]
created: 2026-10-09
updated: 2026-10-09
sources: ["[202604231830]从可观测到可理解用UModel构建Agent原生的代码知识图谱.html"]
---
# token 预算优化输出格式

"token 预算优化输出格式"是 [[code-wiki]] CLI 的输出设计原则：**默认 `--format brief` 面向 Agent token 预算优化，单次 query context 输出 < 500 tokens；完整数据按需走 `--format json`**。这是"工具输出即上下文"思路下，对 Agent 上下文窗口稀缺资源的显式预算管理。

## brief 格式示例（原文 verbatim）

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

设计特征：类型/函数列表截断展示数量与代表项（`.` 表示省略），反向依赖与组件跨界只给模块名——信息密度优先于完备性。

## 在 token 预算主题簇中的位置

与 [[分块token预算推导]]（Harness 分块的预算推导）、[[skill列表token预算]]（Skill 列表的元数据预算）同属 token 预算主题：三者分别控制**工具输出、文档分块、技能清单**三类上下文注入的预算上限。