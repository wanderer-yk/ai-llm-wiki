---
type: overview
title: 项目总览
tags: [总览, agent, 架构设计, 企业智能助手, ai-coding, 重构, 多智能体, 狼人杀, spec-driven, specflow, harness-engineering]
related: [agent-architecture-design, openclaw, claude-code, intent-planning, context-engineering, data-self-iteration, enterprise-intelligent-assistant, ren-ren-dui-qi-ren-ji-dui-qi, ai-you-hao-yan-fa-gui-fan, pre-pr-ji-zhi, agentscope, ai-werewolf-game, specflow, spec-driven-development, blocker-gate, harness-engineering]
created: 2026-06-08
updated: 2026-06-08
---
# 项目总览

本 Wiki 聚焦于**大模型 Agent 的架构设计**、**企业智能办公助手的工程落地**、**AI Coding 时代的大规模重构实践**、**多智能体游戏场景的工程实践**、**规格驱动 AI 开发流程**与**Harness Engineering 工程化方法**六大主题，以六篇核心文章为来源，系统记录构建高效、可观测、低成本 Agent 所需的工程方法论与关键权衡。

## 主题一：Agent 架构设计（来源：《从 OpenClaw 看 Agent 架构设计》）

Wiki 围绕 [[agent-architecture-design|Agent 架构设计的四大决策维度]] 展开：

1. **上下文管理**：涵盖 [[append-only-context|追加式上下文]]、[[compression-strategy|压缩策略]]、[[task-isolation|任务隔离]] 三种模式及其组合。
2. **工具加载**：聚焦 [[prompt-cache-mechanism|Prompt 缓存]] 与动态加载的根本矛盾，记录了 [[prompt-based-tool-injection|Prompt 级注入]]、[[console-vs-mcp-strategy|控制台+MCP 混合]]、[[progressive-tool-loading|渐进式加载]] 等折中方案。
3. **工具查找**：提出 [[skill|Skill]] 概念，按功能维度聚合工具，作为 [[skill-as-knowledge-cache|工具调用知识的缓存层]]。
4. **主循环设计**：对比 [[conversation-driven-vs-task-driven|对话驱动与任务驱动]]，推崇将 [[thought-process-as-first-class-citizen|思考过程作为一等公民]] 的 [[perceive-think-act-loop|感知-思考-行动]] 循环。

## 主题二：企业智能办公助手工程落地（来源：富城《颠覆传统！意图规划+上下文工程+数据自迭代》）

[[fucheng|富城]]（[[ma-shang-xiao-fei-ji-shu-tuan-dui|马上消费技术团队]]）提出[[enterprise-intelligent-assistant|企业智能办公助手]]的三层方法论：[[intent-planning|意图规划]]、[[context-engineering|上下文工程]]、[[data-self-iteration|数据自迭代]]。核心立场是在关键环节用确定性工程替代概率性推理，整体成果为查询准确率93%+。

## 主题三：AI Coding 时代的大规模重构实践（来源：美团业务研发平台团队）

[[mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui|美团业务研发平台团队]]基于31万行代码的AI重构实践，提出[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]方法论。核心洞察：当90%以上代码由AI生成时，决定系统走向的不是速度而是**约束AI的能力**。质量保证体系涵盖 [[pre-pr-ji-zhi|Pre-PR预审]]、[[gao-jie-mo-xing-shen-cha-di-jie-mo-xing|高阶模型审查低阶模型]]、[[ren-ji-xie-zuo-ce-shi-sop|人机协作测试SOP]]。

## 主题四：多智能体游戏场景工程实践（来源：亦盏、望宸《什么？我的狼人杀水平还不如AI？》）

使用 [[agentscope|AgentScope Java 版]]构建支持人机混合对战的 [[ai-werewolf-game|AI 狼人杀游戏]]，系统展示了多智能体框架在信息不对称博弈场景下的六大工程挑战及其对应能力。

## 主题五：规格驱动 AI 开发流程（来源：天玑前端团队《治愈 Cursor AI 编程的"幻觉"？用它就够了！》）

[[tian-ji-qian-duan-tuan-dui|天玑前端团队]]（[[ai-qi-yi-ji-shu-chan-pin-tuan-dui|爱奇艺技术产品团队]]）自研 [[specflow|Specflow]] CLI 工具，通过 [[blocker-gate|Blocker Gate]]、[[dan-zhi-ling-zhuang-tai-ji|单指令状态机]]、[[ssot-dan-wen-dang-ce-lue|SSOT单文档策略]] 实现[[spec-driven-development|规格驱动开发]]，将 [[yan-fa-fan-shi-qian-yi|研发范式前移]] 到需求设计期。

## 主题六：Harness Engineering 工程化方法（来源：数据库团队《别让AI瞎猜了：用Harness Engineering终结无限返工》）

[[shu-ju-ku-tuan-dui|爱奇艺数据库团队]]（同为[[ai-qi-yi-ji-shu-chan-pin-tuan-dui|爱奇艺技术产品团队]]公众号）提出 [[harness-engineering|Harness Engineering]] 方法论，定义harness为"让agent能稳定参与研发的工程安排"。方法概念源自OpenAI，本文为行业实践解读。

### 核心框架

1. **[[harness-wu-yao-su|五要素]]**：任务入口、执行依据、工具边界、验证反馈、结果记录——缺一则返工概率陡增
2. **[[harness-wu-ceng-zhi-ze-mo-xing|五层职责模型]]**：任务编排→执行依据→状态暴露与验证→agent执行→评审收口，工具可替换但职责位置不可缺位
3. **[[san-chong-cai-ce-wen-ti|三重猜测问题]]**：仅靠NL描述时agent同时猜测外观/状态/拆分，导致[[ai-bian-cheng-huan-jue|AI编程幻觉]]
4. **[[cong-prompt-dao-harness|范式转换]]**：从Prompt Engineering（文本级操作）到Harness Engineering（工程级操作），核心金句——"prompt解决这一轮怎么说清，harness解决项目里如何持续做对"
5. **[[qian-hou-duan-san-ceng-jia-gou|前后端三层架构]]**：执行依据层→状态暴露层→交付实现层，三层先后站稳才能让agent角色清晰
6. **[[wu-ceng-yan-zheng-ti-xi|五层验证体系]]**：静态检查→单元验证→链路验证→失败验证→回写验证，验证前置于任务设计
7. **[[harness-san-jie-duan-luo-di-lu-jing|三阶段落地路径]]**：入口可找→任务可复用→重复可机械化
8. **[[agent-san-yuan-ze|Agent三原则]]**：无法访问的知识=不存在、无法执行的工具=没有、无法验证的目标=无法修正

### 实践载体

- 项目文件模板：`AGENTS.md`入口地图、`.agent/PLANS.md`计划协议、`docs/harness/`约束文档、`scripts/harness/`检查脚本
- 开源模板仓库：GitHub SisyphusSQ/harness-template（[[harness-template]]）
- 四条口诀：任务别只留聊天、边界别只靠人记、验证别只停本机、结果别只存本轮对话

## 六大主题的交汇

六篇来源从不同视角探讨了 Agent/AI 工程的核心问题，形成互补与呼应：

- **Specflow与Harness Engineering**构成同组织内最直接的**层级互补**：Specflow（天玑前端团队）提供前端规格驱动工具与[[ssot-dan-wen-dang-ce-lue|SSOT单文档策略]]，Harness Engineering（数据库团队）提供全栈agent工程条件框架与多文件结构。两者的plan.md与PLANS.md功能高度重叠
- **[[blocker-gate|Blocker Gate]]**、[[pre-pr-ji-zhi|Pre-PR预审]]、Harness的review gate都是"人工介入质量关卡"的不同实践
- **[[always-ji-bie-ai-rule|always级别AI Rule]]**、[[gui-fan-luo-di-ai-gong-ju-lian|规范落地AI工具链]]、Harness三阶段路径的阶段3（重复机械化）均指向同一方向——将规范从文档升级为强制执行约束
- **[[ren-ren-dui-qi-ren-ji-dui-qi|人人对齐→人机对齐]]**是贯穿美团、Specflow、Harness Engineering的统一叙事框架
- **[[yan-fa-fan-shi-qian-yi|研发范式前移]]**是所有方法论共同的理念基础——在编码前完成工程准备
- **[[liu-cheng-que-ding-xing|流程确定性]]**是对抗AI编程不确定性的共同手段

## 待探索方向

- Harness Engineering与Specflow的完整对比分析页面
- OpenAI原始Harness Engineering框架的具体内容
- Harness五要素在真实项目中的量化效果验证
- 数据库团队与天玑前端团队方法论的融合可能性
- AGENTS.md与CLAUDE.md/.cursorrules等Agent入口文件的关系
- 更多主流 Agent 的架构拆解以支撑横向对比
- Specflow 2.0 Subagents + Agent Skills 的具体实现进展