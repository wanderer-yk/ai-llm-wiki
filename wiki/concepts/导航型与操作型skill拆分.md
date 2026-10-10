---
type: concept
title: 导航型与操作型 Skill 拆分
tags: [skill, 架构拆分, 渐进式披露, 决策树]
related: [cloudflare导航型skill, cloudflare-deploy, skill决策树加按需加载模式, skill渐进式披露, skill列表token预算]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# 导航型与操作型 Skill 拆分

导航型与操作型 Skill 拆分是 [[青斧]] 在讲解决策树模式时提出的进阶技巧：同一知识域可拆成两个 Skill，防止单 Skill 过载——**导航型**只做选型/方向判断，不涉及任何操作；**操作型**承载认证、命令、故障排除等实际执行内容。

教科书范例来自 Cloudflare 知识域：OpenCode 的导航型 [[cloudflare导航型skill|cloudflare]]（纯决策树，211 行，速查表精髓"导航型 vs 操作型的区别"）与 [[openai-skills|openai/skills]] 的操作型 [[cloudflare-deploy]]（线性+决策树，224 行，主文件 7KB、references/ 展开几十万字）。两者构成"纯决策树 vs 线性+决策树"的同平台对照，同时是 [[skill渐进式披露]] 与 [[skill列表token预算]] 的量化实证载体。

该拆分与 [[渐进式披露替代向量检索]] 的目录即检索路径思想一致：知识检索的压力由"单个巨型 Skill"转移到"入口导航 + 按需进入操作细节"的分层结构上。
