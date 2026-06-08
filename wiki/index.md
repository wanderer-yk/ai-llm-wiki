---
type: overview
title: Wiki 索引
tags: [索引]
related: []
created: 2026-06-08
updated: 2026-06-08
---
# Wiki 索引

## 实体（Entities）

- [[openclaw]] — 文章核心分析对象，代表性大模型 Agent 实现
- [[claude-code]] — Anthropic 推出的命令行 Agent，主要对比对象
- [[anthropic]] — 大模型厂商，提供 Claude 系列与 Prompt 缓存 API
- [[mcp]] — Model Context Protocol，远程服务标准化接口与鉴权协议
- [[skill]] — 技能，按功能维度聚合工具的缓存单元
- [[execute]] — 本地终端执行工具，统一本地能力入口
- [[fucheng]] — 富城，马上消费技术团队成员，企业智能办公助手文章作者
- [[ma-shang-xiao-fei-ji-shu-tuan-dui]] — 马上消费技术团队，文章发布主体
- [[enterprise-intelligent-assistant]] — 企业智能办公助手，大模型基础的企业级办公辅助系统
- [[roberta]] — RoBERTa，110M参数BERT类模型，企业意图分类首选
- [[digital-employee]] — 数字员工，智能助手的下一演进形态
- [[quality-inspection-ai]] — 质检AI，基于通用LLM的回答质量检验模块
- [[mei-tuan-ji-shu-tuan-dui]] — 美团技术团队，微信公众号主体，AI Coding文章发布方
- [[mei-tuan-ye-wu-yan-fa-ping-tai-tuan-dui]] — 美团业务研发平台团队，Agent评测系统负责团队
- [[agent-ping-ce-xi-tong]] — Agent评测系统，承载多模态数据评测与质量控制，31万行代码
- [[longcat]] — LongCat，美团AI模型系列品牌（Flash-Chat/AudioDiT/Next）
- [[agentscope]] — AgentScope，阿里达摩院多智能体框架，支持Java/Python双版本，六大核心能力
- [[agentscope-java]] — AgentScope Java版本，release/1.0.5，基于Spring Boot运行
- [[bailian]] — 百炼，阿里云大模型服务平台（DashScope API）
- [[qwen3-plus]] — qwen3-plus，通义千问模型版本，AgentScope狼人杀代码示例使用
- [[alibaba-zhong-jian-jian]] — 阿里巴巴中间件，微信公众号发布主体
- [[yi-zhan]] — 亦盏，AgentScope狼人杀文章作者之一
- [[wang-chen]] — 望宸，AgentScope狼人杀文章作者之一
- [[werewolf-hitl]] — werewolf-hitl，AgentScope狼人杀人机对战示例项目
- [[dashscope-multi-agent-formatter]] — DashScopeMultiAgentFormatter，百炼平台多人对话格式化器
- [[cursor]] — Cursor，AI编程工具/编辑器，Specflow深度适配对象
- [[specflow]] — Specflow，天玑前端团队自研规格驱动AI开发流程CLI工具，专为Cursor定制
- [[tian-ji-qian-duan-tuan-dui]] — 天玑前端团队，爱奇艺旗下，Specflow研发主体
- [[ai-qi-yi-ji-shu-chan-pin-tuan-dui]] — 爱奇艺技术产品团队，微信公众号发布主体
- [[vibe-coding]] — Vibe Coding（氛围编码），早期AI Coding模式，缺乏标准化约束
- [[openspec]] — OpenSpec，轻量级规格驱动方案，核心为原子化变更
- [[github-spec-kit]] — GitHub Spec Kit，工业级标准化协作协议，Constitution宪章+门控
- [[bmad-method]] — BMAD-METHOD，全能型多代理协作工程框架，角色思维隔离
- [[harness-engineering]] — Harness Engineering，让agent稳定参与研发的工程安排，五要素+五层职责模型
- [[shu-ju-ku-tuan-dui]] — 数据库团队，爱奇艺旗下，Harness Engineering方法论的实践解读方
- [[harness-template]] — harness-template，Harness Engineering开源项目模板仓库（GitHub SisyphusSQ）

## 概念（Concepts）

- [[agent-architecture-design]] — Agent 架构设计的四大决策维度框架
- [[append-only-context]] — 追加式上下文：全量历史发送，只增不减
- [[compression-strategy]] — 压缩策略：逼近窗口上限时压缩上下文
- [[task-isolation]] — 任务隔离：独立上下文窗口避免干扰
- [[prompt-cache-mechanism]] — Prompt 缓存机制：严格前缀匹配
- [[prompt-based-tool-injection]] — Prompt 级工具注入：工具描述移入 messages
- [[console-vs-mcp-strategy]] — 控制台 + MCP 混合工具加载策略
- [[progressive-tool-loading]] — 渐进式工具加载：按需追加保缓存
- [[skill-based-organization]] — 技能维度组织：按功能聚合工具
- [[skill-as-knowledge-cache]] — Skill 作为工具调用知识的缓存层
- [[conversation-driven-vs-task-driven]] — 对话驱动 vs 任务驱动主循环模式
- [[perceive-think-act-loop]] — 感知-思考-行动任务驱动主循环
- [[thought-process-as-first-class-citizen]] — 思考过程作为一等公民，提升可观测性
- [[model-training-bias]] — RLHF 导致的直接回复偏好
- [[architectural-decision-interdependence]] — 四大架构决策的相互关联性
- [[intent-planning]] — 意图规划：精准理解用户意图的方法论
- [[context-engineering]] — 上下文工程：保障信息准确交付的方法论
- [[data-self-iteration]] — 数据自迭代：驱动持续优化的方法论
- [[query-rewriting]] — Query改写：多轮补全、指代消解、长文摘要
- [[small-model-beats-llm-in-classification]] — 精确分类中小模型碾压LLM
- [[entity-merge-wide-table]] — 主体合并宽表：预聚合多系统数据，准确率~100%
- [[faq-conversion]] — FAQ转化：动态数据转静态知识对，准确率98%+
- [[nlp2sql-limitation]] — NLP2SQL局限性：复杂场景<80%不可用
- [[agent-collaboration-delivery]] — Agent协同调用交付：智能规划+精准路由双模式
- [[safety-iron-rule]] — 安全铁律：写操作工具执行前强制授权
- [[conflict-resolution-four-principles]] — 信源冲突裁决四原则
- [[ren-ren-dui-qi-ren-ji-dui-qi]] — 人人对齐→人机对齐：AI Coding核心方法论
- [[ai-you-hao-yan-fa-gui-fan]] — AI友好研发规范：约束AI产出的基础设施
- [[always-ji-bie-ai-rule]] — always级别AI Rule：规范升级为强制执行约束
- [[pre-pr-ji-zhi]] — Pre-PR预审机制：AI多轮自查后提交CR
- [[gao-jie-mo-xing-shen-cha-di-jie-mo-xing]] — 高阶模型审查低阶模型
- [[kua-chang-shang-mo-xing-dui-kang]] — 跨厂商模型对抗审核
- [[ren-ji-xie-zuo-ce-shi-sop]] — 人机协作测试SOP：Human-in-the-loop五步法
- [[ling-pai-qi-zhong-gou]] — 零排期重构：技术债随业务迭代消化
- [[si-ceng-jia-gou]] — 四层架构：Starter/Application/Infrastructure/Common
- [[bian-pai-lei-yu-neng-li-lei]] — 编排类与能力类：职责边界划分维度
- [[zhu-r-da-yang-sop-fen-fa]] — 主R打样→SOP分发→全组并行执行
- [[po-quan-lu-lou-lu-zhi-li]] — PO全链路泄露治理三步法
- [[zhuan-jia-jing-yan-ding-xiang-ai-fu-zhu-pai-cha]] — 专家经验定向+AI辅助排查
- [[jing-yan-jia-zhi-zhuang-yi]] — 经验价值转移：从"能看全"到"能判断什么重要"
- [[gui-fan-luo-di-ai-gong-ju-lian]] — 规范落地AI工具链：不落地就是一纸空文
- [[yan-tong-shi-gong-neng-kai-fa]] — 烟囱式功能开发：缺乏数据模型扩展能力
- [[ji-shu-zhai]] — 技术债：P0/P1分级（业务模型缺陷、DB性能、状态管理等）
- [[ai-werewolf-game]] — AI狼人杀游戏：人机混合对战的社交推理博弈，六大工程挑战
- [[react-paradigm]] — ReAct范式：思考-行动-再思考持续推理循环
- [[msg-hub]] — MsgHub消息频道机制：发布-订阅模式，频道隔离+广播控制
- [[call-and-observe-pattern]] — call/observe双模式：主动发言与被动接收解耦
- [[multi-agent-formatter]] — 多智能体格式化器：消息标记+合并解决LLM三角色限制
- [[role-specific-prompt-strategy]] — 角色专属Prompt策略：博弈论层面的策略选择（悍跳狼/深水狼）
- [[function-calling-structured-output]] — 基于Function Calling的结构化输出：Java POJO→JSON Schema→临时工具→自动验证
- [[agent-interface-polymorphism]] — Agent接口多态：UserAgent/ReActAgent同接口，编排器零改动
- [[sinks-one-async-pattern]] — Sinks.One异步等待模式：Reactor响应式实现Human-in-the-Loop
- [[dual-perspective-architecture]] — 双视角架构：玩家视角（角色过滤）+上帝视角（全量复盘）
- [[werewolf-game-loop]] — 狼人杀游戏循环：夜晚→白天讨论→投票→胜负判定四阶段编排
- [[spec-driven-development]] — 规格驱动开发（SDD）：将模糊需求拆解为机器可理解契约的方法论
- [[ai-bian-cheng-huan-jue]] — AI编程幻觉：AI编程工具生成看似合理但实际错误的代码或建议
- [[blocker-gate]] — Blocker Gate：逻辑阻断门控，前置阶段未达标则硬性阻断后续编码
- [[dan-zhi-ling-zhuang-tai-ji]] — 单指令状态机：`/specflow`一条指令自动寻迹，零心智负担
- [[ssot-dan-wen-dang-ce-lue]] — SSOT单文档策略：`plan.md`集中所有信息，避免跨文件检索
- [[yan-fa-fan-shi-qian-yi]] — 研发范式前移（Shift Left）：将质量保障着力点从编码调试期前推至需求设计期
- [[liu-cheng-que-ding-xing]] — 流程确定性：通过标准化流程对抗业务复杂性
- [[harness-wu-yao-su]] — Harness五要素：任务入口/执行依据/工具边界/验证反馈/结果记录
- [[harness-wu-ceng-zhi-ze-mo-xing]] — Harness五层职责模型：任务编排→执行依据→状态暴露验证→agent执行→评审收口
- [[cong-prompt-dao-harness]] — 从Prompt到Harness的范式转换：文本级操作→工程级操作
- [[san-chong-cai-ce-wen-ti]] — 三重猜测问题：agent同时猜测外观/状态/拆分导致输出不可控
- [[wu-ceng-yan-zheng-ti-xi]] — 五层验证体系：静态检查→单元验证→链路验证→失败验证→回写验证
- [[qian-hou-duan-san-ceng-jia-gou]] — 前后端三层协作架构：执行依据层→状态暴露层→交付实现层
- [[harness-san-jie-duan-luo-di-lu-jing]] — Harness三阶段落地路径：入口可找→任务可复用→重复可机械化
- [[agent-san-yuan-ze]] — Agent工作三原则：无法访问的知识=不存在/无法执行的工具=没有/无法验证的目标=无法修正

## 来源（Sources）

- [[从OpenClaw看Agent架构设计]] — vivo 互联网搜索团队，剖析 Agent 四大架构决策
- [[颠覆传统意图规划上下文工程数据自迭代让企业智能办公助手效能跃升200V10]] — 富城，马上消费技术团队，企业智能办公助手三层方法论
- [[用Agent评测思路管理AICoding31万行代码AI重构的实践]] — 美团业务研发平台团队，31万行Agent评测系统AI重构实践
- [[什么我的狼人杀水平还不如AI]] — 亦盏、望宸，阿里巴巴中间件，AgentScope Java版构建AI狼人杀游戏
- [[治愈CursorAI编程的幻觉用它就够了]] — 天玑前端团队，爱奇艺技术产品团队，Specflow规格驱动AI开发流程
- [[别让AI瞎猜了用HarnessEngineering终结无限返工]] — 数据库团队，爱奇艺技术产品团队，Harness Engineering方法论实践解读

## 查询（Queries）

（暂无）

## 对比（Comparisons）

（暂无）

## 综合（Synthesis）

（暂无）