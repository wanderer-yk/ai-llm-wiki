---
type: source
title: "当整个团队开始 0 人工Coding：一份万字AI Native研发实战手册"
authors: [binxiong]
year: 2026
url: ""
venue: 微信公众号"腾讯技术工程"
tags: [ai-native, openspec, codebuddy, ai-coding, 腾讯, 研发流程, skill]
related: [binxiong, codebuddy, openspec, opsx指令集, 腾讯技术工程, ai-native研发模式, 三大武器库, 活文档机制, bridge-rule, 原子化变更原则, mr双重视角审查]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202604031907]当整个团队开始0人工Coding一份万字AINative研发实战手册.html"]
---
# 当整个团队开始 0 人工Coding：一份万字AI Native研发手册

## 基本信息

- **作者**：[[binxiong]]（腾讯工程师）
- **发布主体**：[[腾讯技术工程]]（微信公众号）/ 腾讯程序员
- **发布日期**：2026年4月3日
- **标签**：原创

## 核心内容摘要

本文记录腾讯某团队从 AI 辅助编码（2023）→ 验证可行性（2025，三个项目：标准软件、产品市场、极光平台）→ 全面 [[ai-native研发模式]] 转型（2026）的完整历程。文章提出 [[openspec]] + [[codebuddy]] 全链路方案，将 AI 从"打字员"升级为"施工队长"，核心主张是 AI 辅助编码的效率提升上限约 50%，瓶颈在协作方式而非模型能力。

## 文章结构

### 一、四大痛点诊断

| 痛点 | 具体表现 | 根因 |
|------|---------|------|
| 人机协作无标准 | 效果完全取决于个人提示词功力 | 无统一 AI 交互规范 |
| 流程断点 | 工程师沦为"人肉翻译器" | 设计工具与编码工具数据隔离 |
| 上下文缺失 | AI 无法主动读取项目代码库和技术规范 | AI 缺乏主动感知能力 |
| 文档脱节 | 文档与代码天然失同步 | 不在同一版本控制系统 |

### 二、OpenSpec "研发契约"定位

OpenSpec 的本质是"机器和人都能理解的标准图纸"，作为 AI 施工的唯一依据。腾讯版 OpenSpec 采用多文件分目录策略（`openspec/changes/[变更名]/proposal.md + design.md + tasks.md + specs/`），与爱奇艺版的 [[ssot单文档策略]]（`plan.md`）存在显著差异。

人的核心角色仅三个：决策、审批、把关，其余全部交给 AI。

### 三、opsx 完整指令集

[[opsx指令集]] 包含 8 条命令：

**核心流程**（4 条）：`explore`（探索讨论）→ `propose`（规划文档）→ `apply`（AI 按图施工）→ `archive`（归档同步）

**扩展指令**（4 条）：`new`（空脚手架）、`continue`（步进式生成）、`ff`（快进补全）、`verify`（AI 代码审计）

关键工程约束：tasks.md 控制在 15 项以内以防 AI 幻觉；apply 时开发者只需做 Code Review。

### 四、三大武器库

[[三大武器库]] 围绕 CodeBuddy 构建统一规范体系：
1. **知识库（知道）**：双通道注入——OpenSpec specs/ 核心记忆 + MCP Knot 辅助记忆；知识库按五分类分源
2. **MCP（连接）**：4 个 MCP 工具连接（TCS Component 前端组件库、[[tapd]] 需求平台、[[iwiki]] 文档平台、极光流水线 CI/CD）
3. **Skills（掌握）**：SOP 封装为 AI 可复用技能包，统一管理于 tcsc-skills 仓库，通过 SkillHub 市场分发

### 五、生产级 Skill 示例

以 openspec-installer 为例深度解剖生产级 Skill 的完整构造（文件结构 SKILL.md + version.json + scripts/ + templates/），从 8 步手动环境搭建（至少 1 小时）压缩为 1 条命令。强调 [[bridge-rule]]（解决 AI 不感知 config.yaml 的指路牌设计）、Token 安全防护、三级降级策略、自举式开发（用 OpenSpec 管理 OpenSpec 自身迭代）。

### 六、团队协同规矩

1. **[[原子化变更原则]]**：一次 Change 只对应一个需求/Bug、tasks≤15 项、小步快跑
2. **规范即文档原则**：禁止绕过 OpenSpec 手写核心逻辑，否则 Spec 沦为"死文档"
3. **[[mr双重视角审查]]**：三维度对照——代码逻辑对 design.md、架构合规对 proposal.md、需求覆盖对 specs/

### 七、程序员角色重定义

AI 不是替代程序员，而是将核心价值从"能写代码"转移为"能指挥 AI 写出正确的代码"，注意力从"怎么实现"上移到"做什么"和"为什么做"。类比：机器码→汇编→高级语言→AI Native。

## 关键技术细节

- **npm 包名**：`@fission-ai/openspec`
- **Skill 仓库**：`ted.aurora/tcsc-skills`（按业务域分类）
- **SkillHub 平台**：`skillhub.dev.jiguang.woa.com`
- **Bridge Rule 版本迭代**：v0.4.2 `alwaysApply:false` 漏判 → v0.4.3 `alwaysApply:true` 彻底解决
- **安装脚本设计**：幂等（已装不重装）、跨平台（macOS/Linux/Windows）、非 TTY Token 交互
- **项目配置**：config.yaml 含技术栈/代码规范风格/核心业务背景三字段，被定义为 AI 生成代码的"宪法"

## 与其他来源的交叉

- 与爱奇艺 [[规格驱动ai开发]] 均以"契约"定位 OpenSpec/Specflow，但文档结构策略存在分歧（腾讯多文件 vs 爱奇艺单文件）
- "人负责审批/把关"呼应美团 [[pre-pr机制]] 和爱奇艺 [[blocker-gate]]
- 活文档机制解决美团/爱奇艺共有的"文档脱节"痛点
- 双通道知识注入呼应马上消费 [[上下文工程]]
- 程序员角色重定义与美团 [[经验价值迁移]] 共鸣
- Skills 的 SOP 封装与美团 [[主r打样-sop分发]] 高度一致

## 量化数据缺失

全文未给出"0人工Coding"的量化成效数据（代码 AI 生成率、效率提升百分比等），与美团 31 万行代码重构的详实数据形成对比。效率提升"约 50%"为作者经验判断而非统计度量。