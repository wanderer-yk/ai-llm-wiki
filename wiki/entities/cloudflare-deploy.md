---
type: entity
title: cloudflare-deploy（Skill）
tags: [skill, cloudflare, 决策树, 渐进式披露, 部署]
related: [openai-skills, cloudflare导航型skill, skill决策树加按需加载模式, 导航型与操作型skill拆分, skill渐进式披露]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# cloudflare-deploy（Skill）

`cloudflare-deploy` 是 [[openai-skills|openai/skills]] 仓库中的一个 224 行 Skill（主文件 7KB），是 [[青斧]] 七个分析对象中的第 2 个，速查表终局模式标签为"线性+决策树"，一句话精髓为"大平台的渐进式披露"（见 [[skill决策树加按需加载模式]]）。

结构为：认证前置 → 按用户意图分类的决策树 → 产品索引表。适用判据为"知识域 10+ 分支且每分支有大量详细文档"。其渐进式披露的量化证据尤为关键：主文件仅 7KB，详细内容放在 references/ 中按需展开到几十万字，LLM 用 read 工具按需读取——是 [[skill渐进式披露]] 最具数据支撑的实例。它还与导航型 [[cloudflare导航型skill|cloudflare]]（OpenCode 出品，纯决策树，211 行）构成同一知识域"操作型 vs 导航型"双 Skill 拆分范例：操作型含认证/命令/故障排除，导航型只做选型（见 [[导航型与操作型skill拆分]]）。
