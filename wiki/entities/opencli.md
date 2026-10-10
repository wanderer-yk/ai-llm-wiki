---
type: entity
title: OpenCLI
tags: [cli工具, 浏览器自动化, api优先, agent工具化, 开源]
related: [明径, 千问AI平台, api优先浏览器自动化, opencli五级认证策略, agent浏览器探索七步工作流, cli录制回放生成, 软件竞争可调用性, mcp]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604140830]浏览器自动化从GUI到OpenCLI.html"]
---
# OpenCLI

OpenCLI 是一个将网站 API 封装为本地命令行工具的 CLI 框架，以 npm 包 `@jackwener/opencli` 形式分发，由文章作者 [[明径]] 封装并开源（作者自述配套 Skill 文件「可在开源项目中自行下载」）。其定位是浏览器自动化的替代路径：不与网页界面较劲，直接抓取并复现页面背后的 API 请求，把「点按钮」替换为「调接口」，从而绕开 GUI 自动化效率低、稳定性差的困境。核心命令体系包括 `opencli list`（列出所有命令）、`opencli cascade`（鉴权策略自动探测）、`opencli record`（录制回放）以及 `opencli {site} {command} {option}` 的统一调用形态。

本页内容均来自 [[千问AI平台]] 2026-04-14 文章《浏览器自动化：从GUI到OpenCLI》，该文声明内容仅代表作者个人观点。

## 核心思路

不跟网页界面较劲，直接抓它背后的 API。浏览器里看到的数据，本质上都是前端从某个接口拿回来的；把接口找出来、把请求复现出来，比点按钮靠谱得多。依据认证难度自动选择接入层级（public → cookie → header → intercept → ui），将 UI 自动化压缩为最后手段。

## 能力全景

- **五级认证策略**：public / cookie / header（Bearer/CSRF）/ intercept（Pinia/Vuex Store Action + XHR 拦截）/ ui，由 `opencli cascade` 自动探测 → 详见 [[opencli五级认证策略]]。
- **七步浏览器探索工作流**：browser_navigate → browser_snapshot → browser_network_requests → browser_click + browser_wait_for → 二次抓包对比 → browser_evaluate 验证 → 写适配器 → 详见 [[agent浏览器探索七步工作流]]。
- **双格式适配器**：pipeline 含 evaluate（内嵌 JS）步骤用 TypeScript（`src/clis/<site>/<name>.ts`）；纯声明式（navigate + tap + map + limit）用 YAML（`src/clis/<site>/<name>.yaml`）；保存即自动动态注册。
- **AI 原生生成 CLI**：explore（深度抓取/自动滚动/拦截请求/识别框架与状态管理）→ 策略选择 → synthesize（生成候选 YAML，模板化 URL、字段映射、参数默认值）→ generate（探索→合成→注册→验证串联，支持目标化选择与回退）。
- **record 录制回放**：浏览器录制-智能回放，对请求序列评分排序+语义分析自动生成 CLI 命令 → 详见 [[cli录制回放生成]]。
- **外部 CLI 集成**：支持现有 CLI 直接集成到 OpenCLI。
- **Skill 驱动生成**：SKILL.md 约束输出路径 `~/.opencli/clis/{site}/{command}.yaml|.ts`（禁止 .js/.json/.md/.txt）、命名规范（site/command 小写连字符）、Pre-Generation Checklist；Quick mode（CLI-ONESHOT.md，URL+描述 4 步）/ Full mode（CLI-EXPLORER.md，探索流程/鉴权决策树/平台 SDK 如 Bilibili `apiGet`、`fetchJson`/tap 调试/级联请求/常见坑）双模式。

## 已知局限（来源自述）

- record 录制引擎仅捕获请求元数据（url, method, body: responseBody），未能完整提取 POST/PUT 等写操作中的 Request Body，因此仅能覆盖只读类接口，自动化闭环在「写入场景」中断。
- 来源全文无量化稳定性/效率数据，所有优势判断均为单作者经验陈述。
- UI 自动化虽被降为 Tier 5，但 API 发现本身仍依赖浏览器工具模拟交互——UI 自动化被否定的是「终态」而非「手段」。

## 应用案例

- 阿里内部「会画平台」CLI 化。
- BOSS 招聘自动化：与候选人沟通、统计招聘数据。

## 相关连接

- 与 [[agent生产落地环境重构论]] 互证：把浏览器操作面 CLI 化属于「改造环境」而非「提升模型」。
- 与 [[coding-agent四特征]]：OpenCLI 人为给业务世界补上「相对封闭/可验证」特征。
- 与 [[mcp]]：探索工作流的 browser_* 工具与浏览器类 MCP 工具同构；OpenCLI 固化为 CLI 的路径可与 [[RAG能力MCP服务化]]、[[mcp工具封装模式]] 对比。
- 与 [[生产级skill]]、[[skill知识三层架构]]、[[skill-command-mcp三层架构]]：SKILL.md 的命名规范+检查清单+双模式是 Skill 工程化新样本。
- 与 [[验证门禁化]]：Pre-Generation Checklist 属于生成前门禁（弱关联）。

## 开放问题

- 开源仓库地址与 `@jackwener` 作者身份尚未经来源确认。
- record 是否会补齐 Request Body 以支持写操作闭环待观察；BOSS「和候选人沟通」的写路径实际实现机制来源未明说。
- 文中配合 OpenCLI Skill 使用 QoderWork 生成 CLI，其与阿里 Qoder IDE 的关系待确认。
