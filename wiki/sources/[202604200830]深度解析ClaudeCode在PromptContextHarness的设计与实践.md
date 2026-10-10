---
type: source
title: "深度解析 Claude Code 在 Prompt / Context / Harness 的设计与实践"
created: 2026-10-10
updated: 2026-10-10
tags: [claude-code, prompt-engineering, context-engineering, harness-engineering, agent, anthropic, 逆向分析]
related: [飞樰, claude-code, openclaw, 千问AI平台, prompt-context-harness三阶段, system-prompt动态组装机制, system-prompt优先级链, cacheScope分级缓存, system-reminder注入机制, claude-md四路径分层, 内外双版本提示词, 函数结果清理机制, microcompact工具白名单, autocompact水位线机制, 九段式结构化摘要模板, memdir结构化记忆系统, harness三层次定位, 六大系统内置AgentTool, verification-agent五大设计哲学, fork-sub-agent机制, 多agent设计四动机, permission-engine三行为模型, sandbox按需隔离, 异步生成器主循环, 六步交互流水线, 错误自愈三机制, 钩子事件四类生命周期分类, 钩子结构化JSON干预三能力, 反蒸馏假工具注入, buddy电子宠物确定性生成]
authors: [飞樰]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247559627&idx=1&sn=7847089f5135e5060953f013fa56fd4f&chksm=e81bee66bb3495809153e1169a3bdf7ad1fe39b0c53d5c2b778f0177759efc9b1156f296638f#rd"
venue: "千问AI平台（微信公众号）"
sources: ["[202604200830]深度解析ClaudeCode在PromptContextHarness的设计与实践.html"]
---
# 深度解析 Claude Code 在 Prompt / Context / Harness 的设计与实践

## 基本信息

| 字段 | 值 |
|------|-----|
| 标题 | 深度解析 Claude Code 在 Prompt / Context / Harness 的设计与实践 |
| 作者 | [[飞樰]]（原创） |
| 发布 | [[千问AI平台]]（微信公众号），2026-04-20 08:30，IP 属地浙江，正文含「阿里妹导读」编辑框 |
| 分析对象 | [[claude-code]]（Anthropic 的 AI Coding Agent CLI） |
| 对比锚点 | [[openclaw]] |

**证据等级声明**：作者自述「本文所分析的所有信息均来自于网络他人整理的公开信息」，全篇属二手逆向/整理资料，非 Anthropic 官方来源。源码级主张（尤其 `anti_distillation: ['fake_tools']` 反蒸馏投毒、Undercover Mode、`permissions.ts` 61KB、`sandbox-adapter.ts` 986 行、钩子"20+"事件等数字）未经官方验证，引用时需打折记录。

## 定位与谱系

本文是作者继《深度解析 OpenClaw 在 Prompt / Context / Harness 三个维度中的设计哲学与实践》之后的续作，分析对象由 [[openclaw]] 切换为 [[claude-code]]；与作者的 HermesAgent 系列（[[sources/[202604230830]深入源码HermesAgent如何实现SelfImproving]]、[[sources/[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践]]）同属「Prompt / Context / Harness 三层解析」脉络。全篇结构：Prompt Engineering（静态与动态信息的组装）→ Context Engineering（压缩与记忆）→ Harness Engineering（运行环境与约束）→ 有趣的彩蛋 → 全篇总结。

## 核心论点

1. **三阶段递进框架**（[[prompt-context-harness三阶段]]）：Prompt Engineering（「如何说」，评分类比 70+ 分）→ Context Engineering（「让 AI 看什么」，80~85 分）→ Harness Engineering（「构建怎样的运行环境」，90~95 分）。评分是思想实验式类比，无评测口径，不可记入量化实证。
2. **总结四主线**：System Prompt 模块化拼装与解耦；指令设计极致明确；上下文压缩算法 + 记忆架构保障长周期运行稳定性；代码生成与工具调用关键链路的校验约束提升 Agent 执行成功率。
3. **时代判断**：行业正从「用大模型」转向「用好大模型」；[[claude-code]] 与 [[openclaw]] 并列为「经过诸多开发者验证的最佳实践 / 技术标杆」。

## Prompt Engineering：静态与动态信息的组装

Claude Code 的 System Prompt 是多层级、动态组装：多文件协同 → 字符串数组 → 发送 Claude API（详见 [[system-prompt动态组装机制]] 与 [[system-prompt优先级链]]）。

**第 1 步：QueryEngine 调用链**

```javascript
QueryEngine.ask()
  → fetchSystemPromptParts()     // 获取默认 prompt + 用户上下文 + 系统上下文
  → buildEffectiveSystemPrompt() // 根据优先级选择最终 prompt
  → query()                      // 发送到 API
```

**第 2 步：三大并行组件**（`queryContext.ts` 的 `fetchSystemPromptParts()`）
1. `defaultSystemPrompt` — `constants/prompts.ts` 的 `getSystemPrompt()`
2. `systemContext` — `context.ts` 的 `getSystemContext()`，获取 Git 状态信息
3. `userContext` — `context.ts` 的 `getUserContext()`，获取 CLAUDE.md 内容 + 当前日期

**第 3 步：默认 System Prompt 数组结构**（`getSystemPrompt()`，静态/动态分块）

```javascript
[
  // ===== 静态部分（可全局缓存）=====
  getSimpleIntroSection(),        // 身份介绍
  getSimpleSystemSection(),       // 系统行为规则
  getSimpleDoingTasksSection(),   // 任务执行指南
  getActionsSection(),            // 操作安全守则
  getUsingYourToolsSection(),     // 工具使用指南
  getSimpleToneAndStyleSection(), // 语气和风格
  getOutputEfficiencySection(),   // 输出效率要求

  // ===== 边界标记 =====
  "__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__",  // 缓存边界线

  // ===== 动态部分（每个用户/会话不同）=====
  session_guidance,          // 会话特定指导
  memory,                    // 自动记忆
  ant_model_override,        // 内部模型覆盖
  env_info_simple,           // 环境信息
  language,                  // 语言偏好
  output_style,              // 输出风格
  mcp_instructions,          // MCP 服务器指令
  scratchpad,                // 临时文件目录
  frc,                       // 函数结果清理
  summarize_tool_results,    // 工具结果总结提示
  numeric_length_anchors,    // 长度锚点（内部版）
  token_budget,              // Token 预算
  brief,                     // KAIROS 简报
]
```

**第 4 步：优先级链**（`utils/systemPrompt.ts` 的 `buildEffectiveSystemPrompt()`）

```markdown
优先级从高到低：
1. overrideSystemPrompt  — 强制覆盖（如循环模式下使用）→ 直接返回，忽略一切
2. Coordinator prompt    — 协调器模式激活时的专用 prompt
3. Agent prompt          — 用户定义的 Agent 的 prompt（替换默认）
4. customSystemPrompt    — 通过 --system-prompt 参数传入的自定义 prompt
5. defaultSystemPrompt   — 上面第3步构建的标准 prompt
另外：appendSystemPrompt 始终追加到最后（除非 override 模式）
```

**第 6 步：缓存分块**（`constants/systemPromptSections.ts` 的 `splitSysPromptPrefix()`，详见 [[cacheScope分级缓存]]）

```javascript
[
  { text: "x-anthropic-billing-header: .", cacheScope: null },    // 归属头（永不缓存）
  { text: "You are Claude Code.",          cacheScope: 'org' },   // 前缀
  { text: "静态内容（边界前）",                cacheScope: 'global' }, // 全局缓存
  { text: "动态内容（边界后）",                cacheScope: null },    // 不缓存
]
```

**静态模块 1–7**

| 模块 | 名称 | 核心内容 |
|------|------|----------|
| 1 | 身份介绍 | `You are an interactive agent that helps users with software engineering tasks...`；双 IMPORTANT 段（授权安全测试/CTF 允许，破坏性技术拒绝；绝不猜测 URL） |
| 2 | 系统行为规则 | 工具在 permission mode 下执行，被拒后不得原样重试；`<system-reminder>` 标签来自系统；疑似 prompt injection 须上报；hooks 反馈视为用户反馈；接近上下文上限时自动压缩旧消息 |
| 3 | 任务执行守则（Doing Tasks） | 先读后改；不过度工程（"Three similar lines of code is better than a premature abstraction"）；不为一次性操作建抽象；只在系统边界校验；安全编码（command injection/XSS/SQL injection/OWASP Top 10）；失败先诊断；卡住才升级提问 |
| 4 | 操作安全守则（Executing actions with care） | 可逆性 + blast radius 评估；删除/强推/发布/发消息类动作默认确认；一次批准不等于永久授权；禁止破坏性捷径（如 `--no-verify`）；"measure twice, cut once" |
| 5 | 工具使用指南（Using your tools) | Read/Edit/Write/Glob/Grep 优先于 Bash；Agent 工具用于并行与保护主上下文；>3 查询才用 Explore；Skill 仅限已列出技能；无依赖调用并行 |
| 6 | 语气和风格（Tone and style） | 默认无 emoji；简短；`file_path:line_number` 引用；`owner/repo#123` 格式；工具调用前不加冒号 |
| 7 | 输出效率（Output efficiency） | 外部版直奔主题极简；内部版（Communicating with the user）完整句子、关键节点主动更新、倒金字塔、避免语义回溯（见 [[内外双版本提示词]]） |

**动态模块 1–11**

| 模块 | 名称 | 触发条件/内容 |
|------|------|---------------|
| 1 | 会话特定指导 | 按启用工具动态生成（AskUserQuestion、`!` 前缀、Agent/Fork 模式、Explore、Skill、Verification Agent 内部 A/B） |
| 2 | 自动记忆 | `loadMemoryPrompt()` 加载 MEMORY.md 等，跨会话记忆 |
| 3 | 环境信息 | 工作目录/git/平台/shell/OS/模型身份/知识截止 2025-05/fast mode `/fast` |
| 4 | 语言偏好 | "Always respond in {语言}，技术术语与代码标识符保持原文" |
| 5 | 输出风格 | 自定义 Output Style 提示词 |
| 6 | MCP 服务器指令 | 已连接 MCP 服务器提供的使用说明（参见 [[mcp]]） |
| 7 | Scratchpad | 会话专属临时目录替代 `/tmp`，隔离于项目、免权限提示 |
| 8 | 函数结果清理 | 旧工具结果自动清除，保留最近 {N} 条（见 [[函数结果清理机制]]） |
| 9 | 工具结果总结提示 | 提醒把重要信息写进回复，防结果被清理 |
| 10 | 长度锚点（内部版） | 工具调用间 ≤25 词，最终回复 ≤100 词 |
| 11 | Token 预算 | 用户指定目标为硬性下限（"+500k"/"2M tokens"），提前停止自动续跑 |

模型 ID 清单（动态模块 3 原文）：Opus 4.6 `claude-opus-4-6`，Sonnet 4.6 `claude-sonnet-4-6`，Haiku 4.5 `claude-haiku-4-5-20251001`。

**CLAUDE.md 四路径分层**（详见 [[claude-md四路径分层]]）

| 路径 | 定位 | Git 策略 |
|------|------|----------|
| `~/.claude/CLAUDE.md` | 个人通用偏好（跨项目全局人设） | 用户维度静态配置 |
| 项目根 `CLAUDE.md` | 项目共享规范（架构/编码规范/构建命令） | 必须提交 Git |
| `CLAUDE.local.md` | 个人私有指令（敏感/个性化信息） | 明确不入 Git |
| `.claude/rules/*.md` | 按文件类型/领域拆分规则，Frontmatter 限定生效路径 | 项目内模块化 |

## Context Engineering：压缩与记忆

**与 OpenClaw 的 .md 文件体系对比**：[[openclaw]] 采用 AGENT.md（总纲）、SOUL.md（灵魂）、IDENTITY.md（身份）、USER.md（主人档案）、TOOLS.md（工具清单）、HEARTBEAT.md（心跳任务）、MEMORY.md（长期记忆 + 每日 memory/日期.md + 检索 + 时间衰减）。两者同为 Markdown 驱动，但 .md 类型按 Agent 定位（Coding Agent vs 私人助理）差异化——记忆定位差异论：Claude Code 偏向记忆「项目文档、参考、用户偏好和反馈」；OpenClaw 记录「对话中的重点历史信息」。

**三层渐进式压缩**（对应既有概念 [[三层上下文压缩]] 与 [[上下文压缩策略]]，细节页：[[microcompact工具白名单]]、[[autocompact水位线机制]]、[[九段式结构化摘要模板]]）：

| 项目 | 值/内容 | 出处 |
|------|---------|------|
| 可压缩工具白名单 | Bash、Read、Grep、Glob（Edit/Write 完整保留） | `COMPACTABLE_TOOLS` |
| 图像 token 估算 | 统一 2000 token | microCompact |
| 微压缩路径 1 | 基于时间：截断超时间阈值旧消息工具输出 | microCompact |
| 微压缩路径 2 | 基于缓存：仅 KV Cache 边界外压缩 | microCompact |
| SM Compact 触发门槛 | Token ≥ 10,000 且文本消息 ≥ 5 条 | `DEFAULT_SM_COMPACT_CONFIG` |
| SM Compact 单次上限 | 40,000 token | `DEFAULT_SM_COMPACT_CONFIG` |
| AutoCompact 水位线 | 13,000 token | `AUTOCOMPACT_BUFFER_TOKENS` |
| 记忆检索上限 | 最多 5 条（Sonnet 判读） | `findRelevantMemories.ts` |

**Memdir 结构化记忆系统**（详见 [[memdir结构化记忆系统]]）：User/Feedback/Project/Reference 四类记忆 + `loadMemoryPrompt` 预算化加载 + Sonnet 充当"图书管理员"的 LLM-in-the-loop 语义检索（≤5 条）。

## Harness Engineering：运行环境与约束

定位（详见 [[harness三层次定位]]）：Prompt = What & How；Context = How Better；Harness = How Controlled；手段为接口（Interface）、钩子（Hooks）、护栏（Guardrails），目标为约束/引导/检验/评估。第一实践为系统级强提醒引导（`wrapInSystemReminder` + `<system-reminder>` 全生命周期注入，见 [[system-reminder注入机制]]）。

- **六大系统内置 AgentTool**（详见 [[六大系统内置AgentTool]]）：General-Purpose / Explore / Plan / Verification / Guide / Statusline Setup，另有隐藏的 Fork Sub Agent（[[fork-sub-agent机制]]）；多 Agent 设计四动机见 [[多agent设计四动机]]；Verification Agent 五大设计哲学见 [[verification-agent五大设计哲学]]。
- **Permission Engine**（详见 [[permission-engine三行为模型]]）：Allow/Deny/Ask 三行为模型；规则优先级链 `settings.json → CLI 参数 → 命令行规则 → session 规则`。
- **Sandbox**（详见 [[sandbox按需隔离]]）：文件系统只读挂载 + Network/PID 命名空间 + 非 root 降级三层隔离；`shouldUseSandbox` 按需排除；Linux 基于 `bubblewrap (bwrap)`。
- **运行时**：`queryLoop` 异步生成器主循环（[[异步生成器主循环]]）+ 六步交互流水线（[[六步交互流水线]]）+ 错误自愈三机制（[[错误自愈三机制]]）。
- **钩子体系**：14 个可见事件分四类生命周期（[[钩子事件四类生命周期分类]]）；结构化 JSON 干预三能力——阻断/篡改/注入（[[钩子结构化JSON干预三能力]]），`TOOL_HOOK_EXECUTION_TIMEOUT_MS` 默认 10 分钟超时保护。

## 有趣的彩蛋

- **Caffeinate**：借用 macOS `caffeinate` 防空闲休眠；5 分钟自动退出 + 每 4 分钟重启；SIGKILL 不触发清理回调故 5 分钟自毁防电脑永不休眠；仅 Mac 独有。
- **反蒸馏**（详见 [[反蒸馏假工具注入]]）：`anti_distillation: ['fake_tools']` 服务端注入假工具定义构成蒸馏投毒防御（**未经官方证实的二手解读**）；精简输出模式一行汇总工具调用 + Thinking Content 丢弃。
- **Undercover Mode**：内部员工为公共/开源项目贡献代码时隐藏 AI 身份，commit 禁现 "Claude Code"、"Co-Authored-By" 及模型代号（未经官方确认）。
- **Dogfooding**：`process.env.USER_TYPE === 'ant'` 为内部功能总开关（与 [[自举式开发]] 呼应但角度不同）。
- **辱骂检测**：正则匹配负面关键词（仅内部员工开放），触发反馈调查而非惩罚——「挫败即改进信号」。
- **continue 消歧**：`continue` 须完整输入才触发，`keep going` 可在句中（词义在代码语境有歧义）。
- **荒诞加载动词**：思考时随机展示 100+ 荒诞动词（如 "Hullaballooing."）。
- **Buddy System 电子宠物**（详见 [[buddy电子宠物确定性生成]]）：`/buddy` 孵化 5 行 × 12 字符 ASCII 宠物，Mulberry32 + UserID 确定性生成；稀有度五档 60/25/10/4/1%；Shiny 1%；五大属性 DEBUGGING/PATIENCE/CHAOS/WISDOM/SNARK；Bones 确定性生成不存储 / Soul AI 孵化生成持久化。作者评论：彩蛋全删不影响功能，但赋予工具人情味与可玩性，反映 Anthropic「严肃中带幽默、技术中带温暖」的企业文化。

## 跨源关联

- 反蒸馏 ↔ [[用户数据不直接训练论]] / [[rl知识蒸馏降本论]]（蒸馏防御攻防两面）
- 钩子 ↔ [[全生命周期hook机制]]（HermesAgent，跨框架对比候选）
- 错误自愈 ↔ [[结构化错误分类自愈体系]]；子 Agent ↔ [[受控子Agent机制]] / [[SubAgent记忆隔离]]
- Verification Agent ↔ [[验证门禁化]]；Permission+Sandbox+Hooks ↔ [[agent-control-plane]]
- 三层压缩 ↔ [[三层上下文压缩]] / [[双压缩范式对比]]；KV 缓存 ↔ [[快照冻结与前缀缓存]]
- 「最佳实践技术标杆」定位 ↔ 既有比较页 [[comparisons/hermes-agent-vs-openclaw-vs-claude-code]]
- 按任务难度分档选模型 ↔ [[高阶模型审查低阶模型]]

## 研究缺口与存疑清单

1. `anti_distillation: ['fake_tools']` 真实性与实际效果——极非常规主张，纯二手源码解读，无官方佐证。
2. Undercover Mode 的真实性与伦理边界无官方确认。
3. 钩子"20+ 种事件"与文中可见 14 个的差额；`processHookJSONOutput` 完整 JSON schema。
4. 源码级数字（`permissions.ts` 61KB、`sandbox-adapter.ts` 986 行、10 分钟超时、60%~1% 稀有度概率）二手可验证性存疑。
5. 未解机制细节：Verification Agent 内部 A/B 机制、MicroCompact 时间路径阈值、函数结果清理 {N} 值、长度锚点仅内部版的原因。
6. 作者前作两篇（《深度解析 OpenClaw 在 Prompt / Context / Harness 三个维度中的设计哲学与实践》《Agent / Skills / Teams 架构演进过程及技术选型之道》）建页评估待系列读完统一决定。
