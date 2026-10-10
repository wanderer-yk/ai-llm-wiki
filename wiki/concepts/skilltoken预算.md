---
type: concept
title: Skill Token 预算
tags: [skill, token预算, 上下文工程, 渐进式披露]
related: [skill知识三层架构, skill渐进式披露, token预算优化输出格式, skill列表token预算, 分块token预算推导]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# Skill Token 预算

Skill Token 预算是 [[青斧]] 在文章 4.4 节与知识三层架构配套给出的量化参考，界定了单个 Skill 在上下文中的合理开销：

| 层级 | Token 预算 | 内容 |
|------|-----------|------|
| Frontmatter | ~100 tokens | name + description |
| 主文件 | 2K-5K tokens | 核心指令 |
| 参考文档（单个） | 1K-3K tokens | 按需加载 |
| 总上下文占用 | <10K tokens | 主文件 + 1-2 个参考文档 |

核心约束：**总上下文占用应 <10K tokens**（主文件 + 1-2 个参考文档），超出即应把内容下沉到 references/ 按需加载——这正是 [[skill渐进式披露]] 的量化判据。与其他 token 预算概念的关系：[[token预算优化输出格式]]（UModel 案例的输出格式压缩）、[[skill列表token预算]]（Claude Code 源码分析中 Skill 列表的元数据开销）、[[分块token预算推导]]（长程任务的分块预算推导）分别覆盖不同层级，本文数值是最小粒度的"单 Skill 预算"参考。

**待溯源**：这组数字（~100 / 2K-5K / 1K-3K / <10K）是作者实测还是转述规范尚未确认（见来源页开放问题 3）。
