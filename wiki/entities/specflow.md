---
type: entity
title: Specflow
tags: [ai编程, 规格驱动开发, cursor, 前端工程化, 爱奇艺]
related: [cursor, openspec, github-spec-kit, bmad-method, 规格驱动ai开发, 单指令状态机, ssot单文档策略, blocker-gate]
created: 2026-06-12
updated: 2026-06-12
sources: ["[202603261200]治愈CursorAI编程的幻觉用它就够了.html"]
---
# Specflow

Specflow 是爱奇艺天玑前端团队自研的**规格驱动 AI 开发流程（SDD）方案**，旨在解决 [[cursor]] AI 编程工具的幻觉问题，实现开发流程标准化与效率提升。该方案融合了 [[openspec|OpenSpec]]（原子化变更）、[[github-spec-kit|GitHub Spec Kit]]（严格门控）、[[bmad-method|BMAD-METHOD]]（角色思维隔离）三者之长，深度适配 Cursor IDE。

## 四大设计哲学

1. **全链路流程闭环**：Specify→Plan→Implement→Archive 四阶段流水线，问题澄清→技术建模→原子化实现→知识归档
2. **严格物理门控**：[[blocker-gate|Blocker Gate]] 硬性阻断机制，需求未对齐时强制停顿，防止"带病"进入开发
3. **[[单指令状态机]]**：开发者仅需输入 `/specflow` 一个入口指令，系统自动检索磁盘文件状态判定当前阶段，实现零心智负担的"自动寻迹"
4. **[[ssot单文档策略|SSOT 单文档策略]]** + [[多agent角色思维隔离|多 Agent 角色思维隔离]]：所有信息集中在 `plan.md`，配合 PM/TL/Dev/Admin 四角色 Prompt 分配实现内生审计

## 四阶段核心工作流

| 阶段 | 命令 | 角色 | 核心动作 | 产物 |
|------|------|------|----------|------|
| Specify | `/specflow-specify` | PM | 扫描代码库提取业务规则、需求澄清 | `ai-docs/{ID}/specify.md` |
| Plan | `/specflow-plan` | 架构师 | 制定 `[F-xx]` 功能契约与执行路径 | `ai-docs/{ID}/plan.md` |
| Implement | `/specflow-implement` | 工程师 | 按 Group 顺序原子化编码、断点 Review | 代码 + Log 存证 |
| Archive | `/specflow-archive` | 知识管理员 | 知识脱水、迁移文档、更新全局索引 | `summary.md` + `ARCHIVE_SUMMARY.md` |

## 核心特性

- **`[Block]`/`[?]` 双级问题标记**：Plan 阶段区分强制阻断类和可选建议类问题，灵活控制门控粒度
- **`[User]` 区域**：`specify.md` 中供人类填写澄清信息的规定区域
- **断点 Review 机制**：Implement 阶段每个 Group 完成后强制停顿展示 Diff，等待人类授权
- **Dry-run 安全机制**：Archive 阶段先预览后确认的归档安全策略
- **业务感知进化**：归档积累使 AI 从"辅助写码"进化为"理解意图"

## 安装与部署

- 通过爱奇艺**私有 npm 镜像源**安装 Specflow CLI
- 前置要求：Cursor IDE（唯一支持）、Node.js ≥ 18.0.0
- 初始化时将 Commands 和 Templates 注入项目 `.cursor` 目录
- 目前**仅限内部使用**，尚未公开/开源

## 三大核心职责

1. **流程标准化**：统一命令与模板消除协同随机性
2. **质量约束力**：与 Cursor Rules 互补——Specflow 定义"如何做"，Cursor Rules 定义"做得好不好"
3. **业务感知进化**：AI 业务理解随需求归档积累动态增长

## 未来规划：Specflow 2.0

定位从"模拟流程"进化为"自治架构"，两大核心方向：

- **Subagents**：角色原子化解耦为独立子代理、上下文精准注入裁剪非必要历史推理、自主验证闭环
- **Agent Skills**：意图驱动流转摒弃手动命令、原子能力封装标准化脚本片段

## 局限性

- 全程无量化效果数据（效率提升百分比、幻觉率下降等指标缺失）
- 工具尚未公开，外部可复用性未知
- 当前版本仍依赖手动触发和角色 Prompt 模拟，尚未实现真正的多代理自治