---
type: concept
title: Agent 浏览器探索七步工作流
tags: [agent工作流, 浏览器自动化, api发现, sop]
related: [opencli, api优先浏览器自动化, opencli五级认证策略, cli录制回放生成, mcp]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604140830]浏览器自动化从GUI到OpenCLI.html"]
---
# Agent 浏览器探索七步工作流

Agent 浏览器探索七步工作流是 [[明径]] 在 [[opencli]] 文章中给出的固定 SOP：AI Agent 使用浏览器工具系统性发现网站底层 API，先探索、后固化，最终为确认的 API 编写 CLI 适配器。这是 API 优先浏览器自动化（[[api优先浏览器自动化]]）原理的核心执行流程。

## 七步流程（源文档原样保留）

| 步骤 | 工具 | 做什么 |
|------|------|--------|
| 0. 打开浏览器 | `browser_navigate` | 导航到目标页面 |
| 1. 观察页面 | `browser_snapshot` | 观察可交互元素（按钮/标签/链接） |
| 2. 首次抓包 | `browser_network_requests` | 筛选 JSON API 端点，记录 URL pattern |
| 3. 模拟交互 | `browser_click` + `browser_wait_for` | 点击"字幕""评论""关注"等按钮 |
| 4. 二次抓包 | `browser_network_requests` | 对比步骤 2，找出新触发的 API |
| 5. 验证 API | `browser_evaluate` | `fetch(url, {credentials:'include'})` 测试返回结构 |
| 6. 写代码 | — | 基于确认的 API 写适配器 |

## 关键设计点

- **两次抓包对比**：步骤 2 与步骤 4 的差集即懒加载触发的深层 API（字幕/评论/关注列表等），这是对纯静态分析的否定——探索必须包含主动模拟交互。
- **先验证后编码**：步骤 5 用 `fetch(url, {credentials:'include'})` 在浏览器内确认返回结构，再进入步骤 6 写适配器，避免基于猜测生成代码。
- **工具同构性**：browser_* 工具与浏览器类 [[mcp]] 工具同构，该工作流可迁移到任何具备浏览器快照/网络请求捕获/JS 执行能力的 Agent 环境。

## 与鉴权策略的关系

工作流发现 API 后，接入层级的取舍由 [[opencli五级认证策略]] 的决策树决定（`opencli cascade` 自动探测），二者构成「探索→固化」的完整闭环；自动批量生产侧则由 [[cli录制回放生成]] 与 SKILL.md 驱动的 CLI 生成承接。
