---
type: concept
title: OpenClaw 工作区 Markdown 文件族
tags: [openclaw, prompt-engineering, markdown, 工作区]
related: [openclaw, openclaw系统提示词23模块, text大于brain落盘原则, 心跳机制heartbeat, markdown多层记忆体系, openclaw双层记忆系统, claude-md四路径分层]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践.html"]
---

# OpenClaw 工作区 Markdown 文件族

OpenClaw 工作区 Markdown 文件族是 [[openclaw]] 用一组 `.md` 文件解耦配置与硬编码、运行时注入 System Prompt 的文件驱动设计。选 Markdown 的三个理由：文件系统操作方便、格式表达力强、grep/Shell 可管理。注入走两条轨道：AGENTS/SOUL/IDENTITY/USER/TOOLS 五个文件经 System Prompt 模块 15 全文直接注入；SKILL.md 则按需渐进式披露。

| 文件 | 定位（作者命名） | 关键机制 |
|------|------|------|
| AGENTS.md | 总纲/骨架 | Session Startup 四步、Text > Brain、Red Lines、行动分级、群聊纪律、心跳 |
| SOUL.md | 灵魂 | 人格/性格/价值观"人物小传"；**修改须通知用户**（人设稳定性+用户知情权）；Continuity："These files *are* your memory" |
| IDENTITY.md | 身份信息 | Name/Creature/Vibe/Emoji/Avatar 外在标识 |
| USER.md | 主人档案 | 用户称呼/偏好/时区；"you're learning about a person, not building a dossier" |
| TOOLS.md | 工具清单 | 环境私账（摄像头/SSH/TTS）；"Skills are shared. Your setup is yours."——技能更新不丢笔记、技能共享不泄露基础设施 |
| HEARTBEAT.md | 心跳任务 | 周期任务清单；空文件=跳过心跳 API 调用 |
| BOOTSTRAP.md | 首次启动 | 一次性引导初始化（"出生证明"），完成后自动删除 |
| BOOT.md | 启动文件 | 每次启动运行，`hooks.internal.enabled`，配合 Hook 机制 |
| MEMORY.md | 长期记忆 | 跨会话记忆；群聊模式不加载（隐私安全隔离）；详见 [[openclaw双层记忆系统]] |

配套文件 `memory/YYYY-MM-DD.md`（每日记忆）与 `memory/heartbeat-state.json`（心跳状态追踪）。

其他关键纪律：**内外行动分级**（内部动作——读/整理/学习/git 提交自己的改动/更新 MEMORY.md——可自主；外部动作——邮件/推文/公开内容/离开机器/不确定的事——先问）；**群聊参与纪律**（回应条件：被点名/能增值/纠正重要误传/被要求总结；沉默条件：人类闲聊/已有人答/只会说"yeah"/对话顺畅；避免 triple-tap；每条消息最多一个 emoji 反应）；**心跳记忆维护**（每隔几天将 `memory/YYYY-MM-DD.md` 中值得长期保留的洞察蒸馏进 MEMORY.md 并清理过时项——"Daily files are raw notes; MEMORY.md is curated wisdom"）。

与 Claude Code 的 [[claude-md四路径分层]]、OpenClaw 长期记忆文的 [[markdown多层记忆体系]] 构成同类机制的跨框架对照。

来源：[[sources/[202604130830]深度解析OpenClaw在PromptContextHarness三个维度中的设计哲学与实践|[202604130830] 深度解析 OpenClaw]]。
