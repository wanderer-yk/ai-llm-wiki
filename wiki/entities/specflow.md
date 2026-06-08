---
type: entity
title: Specflow
tags: [ai-coding, spec-driven, cursor, cli工具, 研发效能]
related: [cursor, spec-driven-development, blocker-gate, dan-zhi-ling-zhuang-tai-ji, ssot-dan-wen-dang-ce-lue, tian-ji-qian-duan-tuan-dui, openspec, github-spec-kit, bmad-method]
created: 2026-06-08
updated: 2026-06-08
sources: ["治愈CursorAI编程的幻觉用它就够了.html"]
---
# Specflow

**Specflow** 是[[tian-ji-qian-duan-tuan-dui|天玑前端团队]]（爱奇艺旗下）自研的规格驱动 AI 开发流程 CLI 工具，专为 [[cursor|Cursor IDE]] 定制。它通过前置规格约束、硬性门控机制和标准化工作流来解决 AI 编程中的"幻觉"问题。

## 核心定位

- **类型**：研发效能 CLI 工具
- **适配环境**：Cursor Chat (Agent Mode)，不支持其他编辑器
- **分发方式**：通过爱奇艺私有 npm 镜像源全局安装
- **前置要求**：Cursor IDE + Node.js ≥ 18 + Cursor Rules
- **安装产物**：注入 `.cursor` 目录的 commands 与 templates

## 设计哲学（四维）

1. **全链路流程闭环**：Specify→Plan→Implement→Archive 四阶段强制对齐
2. **严格物理门控**：[[blocker-gate|Blocker Gate]]——Specify 细节未澄清或 Plan Block 项未回答则强制停顿
3. **单指令状态机**：[[dan-zhi-ling-zhuang-tai-ji|`/specflow` 入口自动寻迹]]，零心智负担
4. **SSOT 单文档策略**：[[ssot-dan-wen-dang-ce-lue|`plan.md` 集中所有信息]]

## 四阶段工作流

### Specify（问题澄清）
- **指令**：`/specflow-specify`
- **角色**：PM
- **动作**：扫描代码库提取业务规则，支持 UI 设计图上传转 HTML/CSS，`[User]` 区域结构化勾选/填写
- **产物**：`ai-docs/{ID}/specify.md`

### Plan（技术建模）
- **指令**：`/specflow-plan`
- **角色**：架构师
- **动作**：制定 `[F-xx]` 功能契约与 Phase 执行路径，引入 `[Block]`（必答阻塞项）与 `[?]`（可选非阻塞项）两级问题分类
- **产物**：`ai-docs/{ID}/plan.md`

### Implement（原子化实现）
- **指令**：`/specflow-implement`
- **角色**：工程师
- **动作**：按 Group 编号顺序原子化编码，每完成一组强制断点展示 Diff 等待人工授权，回写标准化 Log 存证
- **产物**：代码变更 + 开发日志

### Archive（知识归档）
- **指令**：`/specflow-archive`
- **角色**：知识管理员
- **动作**：知识脱水 → 迁移至 `history/{年}/{季}/` → 更新 `ARCHIVE_SUMMARY.md` 全局索引，引入 Dry-run 预览安全机制
- **产物**：`ai-docs/{ID}/summary.md`

## 三大核心价值

1. **流程标准化**：统一命令与模板消除开发者与 AI 协同的随机性
2. **质量约束**：Specflow 定义"如何做"，Cursor Rules 定义"做得好不好"，二者互补
3. **业务感知进化**：归档需求成为 AI "经验值"，AI 业务理解随使用持续增长

## 与社区方案的关系

Specflow 吸收了三家社区方案的核心思想：
- [[openspec|OpenSpec]] → 原子化变更
- [[github-spec-kit|GitHub Spec Kit]] → 门控机制
- [[bmad-method|BMAD-METHOD]] → 角色思维隔离

但在 Cursor 适配性上做了深度定制，解决了社区方案心智负担重、状态易丢失的摩擦问题。

## Specflow 2.0 规划

未来从"模拟流程"进化为"自治架构"：
- **Subagents**：角色原子化（独立子代理仅加载职能指令集）+ 上下文精准注入 + 自主验证闭环
- **Agent Skills**：意图驱动流转（监控文件状态/对话意图自动触发）+ 原子能力封装

## 局限

- 仅支持 Cursor IDE
- 通过私有镜像源分发，是否开源未明确
- 暂无量化提效数据