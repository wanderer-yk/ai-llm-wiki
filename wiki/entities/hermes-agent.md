---
type: entity
title: Hermes Agent
tags: [agent, 自进化, 开源, rl训练, nous-research, 持久运行]
related: [nous-research, 飞樰, 千问AI平台, openclaw, claude-code, 内外双路径自进化, 动态skill生成, 后台审查agent, agent轨迹, sharegpt格式, grpo算法, 批量数据生成, opd机制, rl-cli标准化训练四阶段, 轨迹头尾保护压缩, 比例阈值压缩, 双压缩范式对比, 内外双驱记忆架构, SQLite全量对话持久化, 即时上下文注入, 全生命周期hook机制, 结构化错误分类自愈体系, 受控子agent机制, 插件化生态扩展, 多层安全护栏, harness五位一体, agent发展三阶段, hermes与openclaw与claude-code三方对比]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604240830]深度解析HermesAgent如何实现自进化及其PromptContextHarness的设计实践.html"]
---
# Hermes Agent

Hermes Agent 是 Nous Research（[[nous-research]]，美国开源人工智能研究机构）于 2026 年 2 月底推出的开源自主智能体（autonomous agent）项目，GitHub 组织为 nousresearch，文章发布时称已获 4 万 Star（该数字待经 GitHub 独立验证，见 [[ai工程量化效果声明追踪]]）。其核心卖点是“持久运行（Persistent）”与“自进化（Self-Evolving）”。官网自述定位（据来源文章引述）：“不是 IDE 中的编程 Copilot，也不是封装单一 API 的聊天机器人外壳，而是部署在服务器上的自主智能体，能记住所学内容，运行时间越长能力越强。”

产品特征（据来源）：40+ 内置工具、兼容多主流大模型、内置 Cron 调度器、交互类似 [[openclaw]]（支持第三方消息平台）、官方支持从 OpenClaw 无缝迁移。在来源文章的 [[agent发展三阶段]] 类比中，Hermes 被定位为继早期被动式 Agent、自主 Agent（OpenClaw、[[claude-code]]）之后的第三阶段“自进化 Agent”代表。

## 核心架构：内外双路径自进化

详见 [[内外双路径自进化]]。外路径为 [[动态skill生成]]（任务后复盘沉淀结构化 Skill 文件包，由 [[后台审查agent]] 异步执行三维度复盘）；内路径为手动触发的 RL 训练闭环（[[research-ready训练闭环]]），自进化数据链路总纲为“任务执行→经验记录→Skill 抽象→模型再训练”。

## 组件地图（代码级证据）

| 文件/配置 | 职责 |
|---|---|
| `run_agent.py` | 主运行入口；`_iters_since_skill` 技能催促计数器、`_skill_nudge_interval = 10`、`_spawn_background_review` 后台审查 |
| `agent/trajectory.py` | 轨迹组织与 ShareGPT 转换（`save_trajectory` / `convert_scratchpad_to_think` / `has_incomplete_scratchpad`），见 [[agent轨迹]]、[[sharegpt格式]] |
| `batch_runner.py` | 主力数据工厂（批量合成轨迹），见 [[批量数据生成]] |
| `mini_swe_runner.py` | SWE 垂直领域数据生成（完成信号 `echo "MINI_SWE_AGENT_FINAL_OUTPUT"`） |
| `environments/agentic_opd_env.py` | OPD 环境，见 [[opd机制]] |
| `trajectory_compressor.py` | 离线轨迹压缩（CompressionConfig 三区算法），见 [[轨迹头尾保护压缩]] |
| `agent/context_compressor.py` | 运行时上下文压缩（50% 比例阈值），见 [[比例阈值压缩]]、[[双压缩范式对比]] |
| `rl_cli.py` | RL 标准化训练 CLI（四阶段），见 [[rl-cli标准化训练四阶段]] |
| `/skills/mlops/training/grpo-rl-training/SKILL.md` | GRPO 训练内置 Skill，见 [[grpo算法]]、[[奖励函数设计黄金法则]] |
| `basic_grpo_training.py` | 多维度奖励函数示例，见 [[多维度组合奖励]] |
| `agent/error_classifier.py` | 14 种错误分类与 Recovery Strategy，见 [[结构化错误分类自愈体系]] |
| `tools/delegate_tool.py` | 子 Agent 沙箱（`DELEGATE_BLOCKED_TOOLS` / `MAX_CONCURRENT_CHILDREN = 3` / `MAX_DEPTH = 2`），见 [[受控子agent机制]] |
| `MEMORY.md` / `USER.md` | 内部静态记忆文件，见 [[内外双驱记忆架构]] |
| SQLite | 全量每日对话历史持久化，见 [[SQLite全量对话持久化]] |

## Prompt / Context / Harness 三维度

- **Prompt**：System Prompt 动态拼装（身份→SOUL.md→工具指南+元数据）与 OpenClaw/Claude Code 高度相似；[[模型异构工具引导]] 按模型“性格差异”注入工具使用补丁（`agent.tool_use_enforcement` 四级配置）；[[生态兼容配置迁移]] 兼容 AGENT.md/SOUL.md/USER.md、CLAUDE.md/.cursorrules/.cursor/rules/*.mdc、WhatsApp/Slack 协议。
- **Context**：[[比例阈值压缩]]（50% 触发）、[[即时上下文注入]]（`@` 符号 7 种语法）、[[内外双驱记忆架构]]（`<memory-context>` 标签注入 + Mem0/Honcho/Hindsight/Supermemory 外部记忆）。
- **Harness**：[[全生命周期hook机制]]（9 项 Hook）、[[结构化错误分类自愈体系]]（14 类错误 + Recovery Strategy）、[[受控子agent机制]]（沙箱隔离）、[[插件化生态扩展]]（Mem0/Hunter 等外部记忆组件、自定义工具、Hook 均以插件形式存在）、[[多层安全护栏]]（Prompt 注入检测 + Skill 静态扫描），作者总结为 [[harness五位一体]]。

## 官方链接（来源参考文献）

- 官网：https://hermes-agent.nousresearch.com/
- GitHub：https://github.com/nousresearch/hermes-agent

## 待核实项

- "4 万 Star" 独立验证；"Hunter" 是否为 "Honcho" 误写；子 Agent 嵌套单层散文表述 vs `MAX_DEPTH = 2` 的口径差异；插件接口规范与安全护栏实现细节。