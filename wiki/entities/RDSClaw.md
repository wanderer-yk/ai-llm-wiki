---
type: entity
title: RDSClaw
tags: [openclaw, RDSClaw, 长期记忆, memory-pipeline, 插件, 阿里云, 云服务, 可观测性]
related: ["openclaw", "LoCoMo10", "自进化记忆管线", "两阶段实时记忆管线", "LLM-CRUD记忆整合", "混合召回", "记忆注入不可信标记", "多通道记忆统一管理", "RDSHermes", "原生记忆不确定性链路", "阿里云", "DuckDB", "全链路可观测体系", "观测三视图", "agent-control-plane", "双提取器分流"]
created: 2026-10-10
updated: 2026-10-10
sources: ["[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系.html", "[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html", "[202604151800]读完ClaudeCode源码才发现SkillsMCPRules的区别远没有你想的那么大.html"]
---

# RDSClaw

RDSClaw 在现有来源中有两种口径不同的定位描述，分源保留：

- 据 `[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html`：RDSClaw 是 [[openclaw]] 记忆插件 `openclaw-memory-alibaba-local` 的官方分发产品形态（据来源文章），为 [[openclaw]] 提供长期记忆管线补强——针对原生记忆系统"管线优秀但 LLM 弱约束决策导致效果不稳定"（玄学效果）的问题，以工程化插件逐环节替代或补强不确定性路径。插件以 `alibaba` 命名并兼容阿里云 DashScope API，与 wiki 已有实体 [[RDSHermes]] 呈"RDS + Agent 产品"命名平行关系，是否同属阿里云 RDS 产品线尚未确认（该段归属推测未经来源直接证实）。
- 据 `[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系.html`：RDSClaw 是阿里云提供的云上 claw 产品（面向 [[openclaw]] 类 Agent 工作负载的云托管环境），控制台原生集成 OpenClaw 生态插件；其与 [[阿里云]] 的关联由文末 aliyun.com 试用购买链接佐证。

> 注：两种定位（记忆插件官方分发形态 vs 云上托管产品）是否为同一产品的不同侧面，来源文章均未直接说明。

## 长期记忆插件管线

> 来源：`[202604151800]OpenClaw长期记忆优秀管线与玄学效果.html`

### 三大模块

1. **个人记忆管线**：双提取器分流（[[双提取器分流]]）+ `agent_end` 钩子驱动的[[两阶段实时记忆管线]] + [[LLM-CRUD记忆整合]] + LanceDB 三索引（向量 ANN + BM25 FTS + 标量索引）[[混合召回]]。
2. **自进化记忆管线**：从 Assistant 消息提取三类目标（learnings/errors/feature_requests），LLM/正则双提取，向量去重 ≥ 0.92，`before_prompt_build` 召回注入，定位"让 Agent 越用越好"（详见 [[自进化记忆管线]]）。
3. **评测证据**：[[LoCoMo10]] 总体 58.18% → 72.08%（+13.90%，加权汇总口径），事实查询 +28.50% 最大、推理性 +21.60%。

### 开箱即用特性（第十章原文）

| 特性 | 说明 |
|------|------|
| 零配置启动 | 安装即用，LLM 提取和向量索引开箱可用 |
| 本地 + 远程双模式 | 本地 GGUF 嵌入模型（离线可用）或远程 DashScope 兼容 API |
| 多通道覆盖 | 钉钉、飞书、企业微信——跨群对话记忆统一管理（[[多通道记忆统一管理]]） |
| 安全保障 | 记忆注入自动标记「不可信历史数据」防 prompt 注入（[[记忆注入不可信标记]]）；敏感信息硬编码排除 |

运营信息：钉钉技术交流群群号 170415008314。

### 与原生系统的关系

互补而非替换：插件不改变底层 LLM，仅通过记忆管线工程优化在 [[LoCoMo10]] 获得近 14 个百分点提升；九维差异（提取时机/方式、演进方式/周期、去重、矛盾处理、时间衰减、存储后端、召回方式）与六项不确定性补强映射详见来源页与 [[原生记忆不确定性链路]]。

## 云上产品能力与可观测集成

> 来源：`[202604011800]OpenClawObservability基于DuckDB构建OpenClaw的全链路可观测体系.html`

### 控制台可观测插件集成

- [[RDSClaw]] 控制台直接集成 `openclaw-observability` 可观测插件，开箱即用。
- 支持接入云上 RDS [[DuckDB]]，并支持本地数据迁移上云（本地单文件 → 云上 RDS 的演进路径）。

### 云上 RDS DuckDB 三优势（文章口径）

1. **稳定性**：备份/容灾/高可用。
2. **多租户管理**：租户隔离/权限控制/资源配额。
3. **弹性性能**：应对流量波动查询峰值。

（产品能力陈述，带营销色彩；原文同时提示可进一步建设统一数据治理与审计体系——监控/分析/归档/合规闭环，与 [[agent-control-plane]] 的审计主题呼应。）

### 部署演进路径

该来源第七章提出三阶段演进：**本地单文件 → 云上 RDS → 统一数据治理审计闭环**（按分析结论并入本页呈现，不单独建页）。

### 试用与规格参数（原文保留）

- 试用链接：`https://yaochi-buy.aliyun.com/rds-ai-deploy?request=%7B%22payType%22:%22Postpaid%22,%22rds_agent_class%22:%22mysql.x2.large.9cm%22%7D`
- URL 参数：payType=Postpaid，rds_agent_class=mysql.x2.large.9cm
- 阿里云 DuckDB 分析实例帮助文档：`https://help.aliyun.com/zh/rds/apsaradb-rds-for-mysql/duckdb-analysis-instance/`

## 待验证与开放问题

据长期记忆来源：

- 与 [[RDSHermes]] 是否同属阿里云 RDS 产品线（DashScope 兼容为旁证）。
- prompt 注入防御的完整机制（标记之外的过滤/隔离细节）与敏感信息排除清单。
- 混合召回融合策略（是否 RRF 或加权）、Evergreen 在 LanceDB 中的实现方式。

据可观测来源：

- 产品全称与定位细节文中未详述。
- `mysql.x2.large.9cm` 规格含义未解释。
- DuckDB 分析实例与 RDS for MySQL 的产品关系未展开。