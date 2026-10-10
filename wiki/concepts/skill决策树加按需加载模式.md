---
type: concept
title: skill 决策树 + 按需加载模式（模式 2）
tags: [skill, 设计模式, 决策树, 渐进式披露]
related: [cloudflare-deploy, cloudflare导航型skill, 导航型与操作型skill拆分, skill渐进式披露, skill知识三层架构, 模式选择决策树, skill线性流程模式]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604270830]工作流的Skill怎么写从7个顶级Skill中提炼的模式与最佳实践.html"]
---
# skill 决策树 + 按需加载模式（模式 2）

决策树 + 按需加载模式是 [[青斧]] 归纳的 5 种核心设计模式之二，适用于大型平台选型、产品导航、问题诊断等"在大量选项中帮用户选对方向"的场景。代表案例为 [[openai-skills|openai/skills]] 的 [[cloudflare-deploy]]（224 行，主文件 7KB），一句话精髓"大平台的渐进式披露"。

**结构**：认证前置 → 按用户意图分类的决策树 → 产品索引表。

**关键技巧**：用户意图分类——用 "I need to run code" 等用户语言而非 "Compute products" 技术术语组织决策树节点，LLM 定位更快；渐进式披露——主文件仅 7KB，详细内容放 references/ 按需展开至几十万字（[[skill渐进式披露]] 的量化实证，与 [[skill知识三层架构]] 一致）。

**选择判据**：知识域有 10+ 分支且每分支有大量详细文档。**进阶**：同一知识域可拆为导航型 + 操作型两个 Skill 防止单 Skill 过载（见 [[导航型与操作型skill拆分]]；OpenCode 的 [[cloudflare导航型skill|cloudflare]] 即纯决策树导航型实例，211 行）。
