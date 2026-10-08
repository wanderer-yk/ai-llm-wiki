---
type: entity
title: web-video-presentation
tags: [skill, claude-code, 视频生成, workflow, 开源]
related: [ConardLi, garden-skills, claude-code, MiniMax, MMX-CLI, 四阶段两检查点流水线, 分阶段文档按需加载, 文件化工作记忆, 并行开发隔离机制, 反馈修复最小切片, 硬性自检规则, 三级评审执行方式, 三种播放模式, 流水线复用模式, 生产级skill]
created: 2026-10-08
updated: 2026-10-08
sources: ["[202605090830]Harness实践让Agent自动制作知识讲解视频.html"]
---
# web-video-presentation

web-video-presentation 是 ConardLi 开发并开源的 Claude Code Skill，用于把单篇技术文章自动转化为带分步视觉演示与口播音频的知识讲解视频网页。Skill 托管于 garden-skills 仓库的 `skills/web-video-presentation` 路径，解压放入 `.claude/skills/` 后以 `/web-video` 触发，其他支持 Skill 的 Agent（如 Cursor、Codex）亦可替代。

## 核心架构（Harness 六核心的 Skill 级落地）

- **流水线**：[[四阶段两检查点流水线]]——内容编写→Plan 检查点→开发（首章人验收为基准）→Audio 检查点→音频合成（TTS）→录屏。
- **上下文**：[[分阶段文档按需加载]]——SCRIPT-STYLE.md / OUTLINE-FORMAT.md / CHAPTER-CRAFT.md / THEMES.md / AUDIO.md / RECORDING.md 六份文档按阶段读取。
- **状态与记忆**：[[文件化工作记忆]]——`outline.md`（结构边界）+ `script.md`（叙事节奏）+ `article.md`（信息密度）。
- **工具与并行**：无特殊工具，仅用好文件读写；[[并行开发隔离机制]]（独立文件夹 + 独立 CSS 前缀 + 主题 token 兜底 + 风格不强求一致）使架构天然支持多 Agent 并行编写。
- **约束与恢复**：[[反馈修复最小切片]]（禁止重做整章）+ 硬性自检 + 三级评审（[[三级评审执行方式]]）。

## 实战终态参数

| 项目 | 值 |
|------|-----|
| 能力上限 | 单篇文章 → 13 章节、100+ 步骤的 16:9 讲解网页 |
| 本次实战 | 《一封邮件发出后的 600 毫秒》→ 50+ 步 |
| 并行上限 | 最多同时 3 个 Agent（作者经验值），须强模型调度 |
| 音频 | MiniMax 两步合成（清单人眼校对→串行逐条）、断点续跑、文件名=步骤编号 |
| 出片 | 三种播放模式 + 浏览器全屏 + OBS 录屏 |
| 归档 | 全部资产入版本控制，换文章重跑流水线 |

## 定位

该 Skill 是 [[生产级skill]] 谱系中"设计+环境+实战+出片+归档+开源分发"全链路证据齐备的完整个案，体现了 [[Skill级Harness论]]：Skill 也是 Harness，只是层级为"Skill 级协作协议"而非"工业级运行系统"。