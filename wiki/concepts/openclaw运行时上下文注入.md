---
type: concept
title: OpenClaw 运行时上下文注入
tags: [openclaw, 上下文管理, 上下文注入, 工作区]
related: [openclaw, openclaw工作区md文件族, 模型记忆与业务上下文记忆分离, openclaw双层记忆系统, prompt极简主义]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202603190830]深入理解OpenClaw技术架构与实现原理上.html"]
---

# OpenClaw 运行时上下文注入

OpenClaw 运行时上下文注入指 [[openclaw]] 在每次运行时将工作区文件、沙箱信息等静态与动态内容组装进模型上下文的机制。**Context 定义**：一次运行中发送给模型的所有内容，四类构成——①系统提示词；②工具列表+描述；③Skills 列表（**仅元数据**）；④工作区位置+时间+运行时数据。**Context ≠ Memory**：记忆可持久化到磁盘，Context 仅当前窗口内容。来源：[[sources/[202603190830]深入理解OpenClaw技术架构与实现原理上|深入理解OpenClaw技术架构与实现原理（上）]] 3.6.1、3.6.6–3.6.8 节。

## 工作区默认注入文件七件套（存在才注入）

| 文件 | 语义 |
|------|------|
| AGENTS.md | 项目规则 |
| SOUL.md | 角色定义 |
| TOOLS.md | 工具指南 |
| IDENTITY.md | 身份信息 |
| USER.md | 用户偏好 |
| HEARTBEAT.md | 心跳状态 |
| BOOTSTRAP.md | 首次运行引导 |

该清单与 [202604130830] 文章一致，使 [[openclaw工作区md文件族]] 获得**第二个独立来源**支撑（双源互证）。

## bootstrap 注入双上限

```json
{
  "agents": {
    "defaults": {
      "bootstrapMaxChars": 20000,
      "bootstrapTotalMaxChars": 150000
    }
  }
}
```

单文件 20000 chars、总量 150000 chars。与剪枝保护中的"bootstrap 免剪"（见 [[openclaw上下文剪枝]]）构成"注入限量 + 剪枝免剪"的成对设计。

## 压缩后关键章节重注入

`src/auto-reply/reply/post-compaction-context.ts`：压缩完成后从 AGENTS.md 重新注入 `## Session Startup` 与 `## Red Lines` 两个章节，目的是"确保模型在压缩后仍遵循关键规则"——即承认压缩会丢失规则，系统主动补偿。

## 沙箱上下文五要素

`src/agents/sandbox/context.ts` 为沙箱会话提供：容器信息（containerName, containerWorkdir）、工作区映射（workspaceDir, agentWorkspaceDir）、Docker 配置（docker）、工具权限（tools）、浏览器桥接（browser, fsBridge）。

## 检查与调试命令（3.6.7）

| 命令 | 作用 |
|------|------|
| `/status` | 快速查看窗口占用率 + 会话设置 |
| `/context list` | 查看注入文件大小、工具 schema 大小 |
| `/context detail` | 详细分解各组件大小 |
| `/usage tokens` | 每次回复显示 token 使用量 |
| `/compact` | 手动触发压缩 |

## 关键配置汇总（3.6.8，verbatim）

```json
{
  "agents": {
    "defaults": {
      "contextTokens": 200000,
      "bootstrapMaxChars": 20000,
      "bootstrapTotalMaxChars": 150000,
      "compaction": {
        "mode": "auto",
        "targetTokens": 0.7
      },
      "contextPruning": {
        "mode": "cache-ttl",
        "ttl": "5m",
        "keepLastAssistants": 3,
        "softTrimRatio": 0.3,
        "hardClearRatio": 0.5
      }
    }
  },
  "models": {
    "providers": {
      "anthropic": {
        "models": [
          { "id": "claude-sonnet-4", "contextWindow": 200000 }
        ]
      }
    }
  }
}
```

## 关联

- [[openclaw工作区md文件族]]：七文件族的母概念页（双源互证）
- [[模型记忆与业务上下文记忆分离]]：Context ≠ Memory 的二分是该分离论的架构级印证
- [[openclaw双层记忆系统]]：记忆模块不在上篇，预期归属《深入理解OpenClaw技术架构与实现原理（下）》核证
- [[prompt极简主义]] / [[渐进式披露替代向量检索]] / [[skill轻量索引按需加载]]：Skills 仅以元数据进入 Context 是三概念的直接源码证据
- [[openclaw上下文压缩流水线]]：重注入机制是压缩流水线的补偿环节
- [[openclaw三层沙箱纵深防御]]：沙箱上下文五要素为沙箱模块预埋（完整拆解预期在下篇）

## 开放问题

- `bootstrapTotalMaxChars: 150000` 恰为默认 `contextTokens: 200000` 的 75%，与工具结果守卫 0.75 headroom 数值重合——是统一预算原则还是巧合，待核
- `## Session Startup` / `## Red Lines` 是否为用户需在 AGENTS.md 中约定的固定章节名；重注入是否还有其他章节
