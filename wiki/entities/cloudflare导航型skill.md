---
type: entity
title: cloudflare（导航型 Skill）
tags: [skill, cloudflare, 决策树, opencode]
related: [cloudflare-deploy, 导航型与操作型skill拆分, skill决策树加按需加载模式, skill设计模式五加一模式对比]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# cloudflare（导航型 Skill）

本页所述 `cloudflare` 是一个 Skill 文件（而非 Cloudflare 公司本身）：由 OpenCode 出品的导航型 Skill，纯决策树结构，共 211 行，是 [[青斧]] 七个分析对象中的第 3 个。其定位是"帮用户在 Cloudflare 大量产品中选对方向"的选型导航——只做选型、不涉及操作，与操作型的 [[cloudflare-deploy]]（认证/命令/故障排除）构成同一知识域"导航型 vs 操作型"双 Skill 拆分的教科书范例。其 frontmatter 贡献了 `references`（声明最重要的参考文档）这一扩展字段的来源实例。一句话精髓为"导航型 vs 操作型的区别"。与 [[cloudflare-deploy]] 的对照还呈现"纯决策树 vs 线性+决策树"的同平台差异（数据源：[[sources/[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践]] 第八章速查表）。
