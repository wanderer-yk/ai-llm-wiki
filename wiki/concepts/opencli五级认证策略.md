---
type: concept
title: OpenCLI 五级认证策略
tags: [鉴权, 决策树, 浏览器自动化, api优先]
related: [opencli, api优先浏览器自动化, agent浏览器探索七步工作流, cli录制回放生成]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604140830]浏览器自动化从GUI到OpenCLI.html"]
---
# OpenCLI 五级认证策略

OpenCLI 五级认证策略是 [[opencli]] 框架提供的鉴权降级决策框架：按接入成本从低到高依次为 public → cookie → header（Bearer/CSRF）→ intercept（Pinia/Vuex Store Action + XHR 拦截）→ ui（UI 自动化，最后手段），并使用 `opencli cascade` 命令自动探测应采用的层级。它是该来源中最具复用价值的工程框架，可作为任何「Agent 调用未开放 API 的网站」场景的鉴权决策参考。

## 决策树（源文档原样保留）

使用命令：`opencli cascade https://api.example.com/hot`

```
直接 fetch(url) 能拿到数据？
  → ✅ Tier 1: public（公开 API，不需要浏览器）
  → ❌ fetch(url, {credentials:'include'}) 带 Cookie 能拿到？
       → ✅ Tier 2: cookie（最常见，evaluate 步骤内 fetch）
       → ❌ → 加上 Bearer / CSRF header 后能拿到？
              → ✅ Tier 3: header（如 Twitter ct0 + Bearer）
              → ❌ → 网站有 Pinia/Vuex Store？
                     → ✅ Tier 4: intercept（Store Action + XHR 拦截）
                     → ❌ Tier 5: ui（UI 自动化，最后手段）
```

## 各层级要点

- **Tier 1 public**：公开 API，无需浏览器，成本最低。
- **Tier 2 cookie**：最常见层级，在 evaluate 步骤内以 `fetch(url, {credentials:'include'})` 携带 Cookie 请求。
- **Tier 3 header**：需附加 Bearer / CSRF 等 header（典型示例：Twitter 的 ct0 + Bearer）。
- **Tier 4 intercept**：利用前端框架状态层（Pinia/Vuex Store）的 Store Action + XHR 拦截获取数据。
- **Tier 5 ui**：仅当以上全部失败时退回 UI 自动化，是明确的最后手段。

## 设计含义与细微之处

- **成本排序即优先级**：决策树严格按「脱离浏览器的程度」排序，越靠前越稳定、越快；UI 自动化被定位为兜底而非首选，与 [[api优先浏览器自动化]] 的立场一致。
- **UI 自动化被否定的是「终态」而非「手段」**：探索工作流（[[agent浏览器探索七步工作流]]）本身仍依赖 browser_click/snapshot 做 API 发现——立场否定的是把 UI 自动化当作最终接入方式。
- **与格式选型的联动**：鉴权层级同时决定适配器格式——简单场景（Cookie/Public auth）用 YAML，复杂场景（Intercept/Header auth、多步逻辑）用 TypeScript（见 [[opencli]] 实体页的 SKILL.md 规范）。
