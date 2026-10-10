---
type: concept
title: skill 生命周期元数据
tags: [hermes-agent, skill, 展望, 生命周期管理]
related: [hermes-agent, 动态skill生成, 全生命周期hook机制, skill组合成工作流, skill创建透明度]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604230830]深入源码HermesAgent如何实现SelfImproving.html"]
---
# skill 生命周期元数据

skill 生命周期元数据是来源文章作者对 [[hermes-agent]] Skill 管理的**展望方向**：当前 SKILL.md 的 YAML frontmatter 仅含 `name`、`description`、`version` 三个静态字段，缺乏使用情况的记录；作者提议增加 `last_used`（最近使用时间）、`use_count`（使用次数）、`success_rate`（成功率）三个使用统计字段，即可低成本实现 Skill 的**自动降权、归档与过时检测**。

**⚠️ 属性标注：作者展望（"如果能……"句式），非 Hermes 已实现特性；该三字段是否已有源码实现待核验。**

## 字段清单（文章原文）

| 状态 | 字段 | 作用 |
|------|------|------|
| 现有 | `name` | Skill 名称 |
| 现有 | `description` | 一句话描述（供轻量索引） |
| 现有 | `version` | 版本号 |
| 提议 | `last_used` | 最近使用时间 → 过时检测 |
| 提议 | `use_count` | 使用次数 → 自动降权/归档依据 |
| 提议 | `success_rate` | 成功率 → 质量降权依据 |

## 动机

配合系统提示词 "Skills that aren't maintained become liabilities" 的维护责任要求：没有使用数据，Skill 只能无限累积，失效 Skill 反而成为负资产；有了元数据即可让 Skill 库自我新陈代谢。

## 关联

与 [[动态skill生成]]（0424 飞樰文）、[[全生命周期hook机制]] 同族互补：动态生成解决"Skill 从哪来"，生命周期元数据解决"Skill 如何退役"。