---
type: source
title: "从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程"
authors: [seanguo]
year: 2026
url: ""
venue: 腾讯技术工程（微信公众号）
tags: [agentic-engineering, claude-code, skill, mcp, 后台开发, vibe-coding, 腾讯]
related: [seanguo, agentic-engineering, skill-command-mcp三层架构, 十一阶段后台开发流程, prompt-and-pray, superpowers插件, claude-code, galileo, knot, dot-agents]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604171736]从VibeCoding到AgenticEngineering重构后台开发全流程.html"]
---
# 从 Vibe Coding 到 Agentic Engineering：重构后台开发全流程

**作者**：seanguo（腾讯开发者）
**发布平台**：[[腾讯技术工程]]（微信公众号），署名栏目：[[腾讯程序员]]
**发布日期**：2026-04-17
**原创标识**：是，IP 属地广东

## 核心论点

Vibe Coding（"提示即祈祷"/[[prompt-and-pray]]）仅适用于原型验证，生产环境必须升级为 Agentic Engineering——人在关键节点审核、AI 在结构化流程中自主执行。核心分工：人定义目标/约束/质量标准，AI 负责在结构化流程中自主执行规划/编码/测试/迭代。

## [[十一阶段后台开发流程]]

基于 [[claude-code]] + [[skill-command-mcp三层架构]] 的完整后台开发流程：

| 阶段 | 名称 | 核心工具 | 人工干预 |
|------|------|----------|----------|
| ① | 需求获取与分支初始化 | `pm-dev` Skill | 口述需求或提供 PM URL |
| ② | 交互式需求澄清 | `superpowers:brainstorming` + [[knot]] MCP | 回答 2-3 个问题（~5min） |
| ③ | 制定实施计划 | `superpowers:writing-plans` | 审核计划（~3min） |
| ④ | 并行执行开发任务 | `superpowers:executing-plans`（Subagent-Driven） | 几乎无需干预（~10min） |
| ⑤ | 代码自审 | `code-review` Skill | 选择修复方式（~2min） |
| ⑥ | 编译部署到测试环境 | `dtools` Skill | 确认参数（~3min） |
| ⑦ | 日志排查与调试 | `galileo-log-query` Skill | 发送测试请求（半自动） |
| ⑧ | 创建 MR | `/create-mr` Command | 无 |
| ⑨ | AI 辅助代码评审 | `/review-mr` Command | 逐一判断评审意见（~3min） |
| ⑩ | 修复评审意见 | `/fix-mr` Command | 确认修复方案（~3min） |
| ⑪ | 合入发布 | — | 点 Merge + 灰度发布（纯人工） |

11 个阶段中仅 4 个需要人工主动干预，开发者角色从"执行者"变为"审核者"。

## 工具体系：[[skill-command-mcp三层架构]]

### Skill 层（8 个）
核心业务逻辑载体，含独立工具权限白名单和执行流程：`pm-dev`、`git-workflow`、`code-review`、`dtools`、`galileo-log-query`、`git-context`、`wiki-doc`、`service-analyzer`

### Command 层（5 个）
轻量级路由入口（薄壳），自然语言可等价触发：`/commit`、`/create-mr`、`/review-mr`、`/fix-mr`、`/analyze-codebase`

### MCP Server 层（5 个）
通过 MCP 协议连接外部平台 API，配置一次全局生效：GitPlatform、PM、Galileo、KnowledgeBase、InternalWiki

### Superpowers 插件
提供结构化工作流 Skill（brainstorming→writing-plans→executing-plans），构成"理解→计划→执行"的强制纪律链，防止 AI 跳过关键步骤自由发挥。

## 关键设计决策

1. **Command 薄壳设计**：每个 `/xxx` Command 仅一行代码委托给 Skill，自然语言与显式命令效果等价
2. **Skill 组合复用原则**：Skill 间链式调用，`git-context` 被多模块复用为前置准备
3. **Superpowers 纪律强制**：brainstorming/writing-plans/executing-plans 是强制流程而非可选建议
4. **MCP 透明化**：用户无需感知 MCP 调用细节，Skill 自动封装

## 关键实践洞察

### 链式调用流水线
`pm-dev`（需求创建）→ `brainstorming`（需求澄清）→ `writing-plans`（计划制定），形成自动流水线

### 两级代码审查
阶段 5 自审（消灭格式/命名/规范类低级问题）vs 阶段 9 正式评审（站在 reviewer 角度做行级评论）

### 跨模型审查
用不同模型审查代码（如 Claude 写的代码用 Codex/Gemini 审查），避免同模型思维盲区。Claude 行号定位准确性优于 DeepSeek。

### AI 规律发现
AI 自行推测出 PM 系统 long ID 与 short_id 的转换关系，这是作者作为长期用户都未发现的规律。

### 评审修复闭环
`/fix-mr` 实现评审→修复→编译验证→提交推送→回复评论的全自动闭环。

### 人工守住发布底线
合入和灰度发布涉及灰度策略和线上风险，是唯一明确不交给 AI 的阶段。

### 防上下文爆炸机制
`galileo-log-query` 内置默认 limit 50 和 level:error 缩小范围策略，避免日志撑爆 Agent 上下文窗口。

## 代码审查四级体系

`code-review` Skill 内置 Golang 专项审查：Critical（空指针/SQL 注入/数据竞争/资源泄漏/循环 defer）→ Major（错误处理不规范/嵌套超 4 层/switch 缺 default/并发安全）→ Minor（命名/import 顺序/魔法数字/函数超 80 行）→ Suggestion（lo 简化集合/copier 简化结构体/Table-Driven Tests）

## 配置目录结构

`~/.claude-internal/` 标准布局：`CLAUDE.md`（全局指令/代码规范/偏好）、`settings.json`（权限白名单/模型选择/插件启用）、`commands/`（斜杠命令）、`skills/`（技能库）

## 真实案例

RedeemReward 接口数据上报逻辑变更：Go mod 依赖更新、结构体扩展、接口逻辑重构。4 个 Task 并行执行（49s~6m25s），自动生成 3 个 Conventional Commits。AI 代码评审实战案例：+252 行 MR 检出 9 个问题（C1/M4/m2/S2），含 goroutine 永久阻塞、竞态窗口等 Critical 级问题。

## 未兑现承诺

文章在区块 6 承诺文末给出"Token 十大技巧"，但全文未兑现，构成内容完整性缺失。

## 与 Wiki 已有知识的关系

- 与 [[sources/[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册|[202604031907]binxiong 的 AI Native 研发实战手册]]：同公众号、同类主题，binxiong 提出 [[opsx指令集]] 和 [[三大武器库]]，seanguo 提出 Skill/Command/MCP 三层架构，可能是同一团队不同成员的互补/演进实践
- 与 [[sources/[202605071734]十年老技术开发的AIAgent探索之路|[202605071734]zhiyuanfu 的 24h 打工人]]：同属腾讯 AI 研发方法论系列
- [[agentic-engineering]] 的工程化约束思路与爱奇艺 [[harness-engineering]] 跨公司趋同
- 结构化工作流 Skill 的"先理解再动手→先计划再执行→有检查清单"与 [[研发范式前移]] 一致
- 两级代码审查设计（自审 vs 正式评审）与美团 [[pre-pr机制]] 概念对应