---
type: concept
title: Harness 边界移动论
created: 2026-10-09
updated: 2026-10-09
tags: [harness-engineering, 长程任务, 方法论演化, 模型能力边界]
related: [harness-engineering, harness基础设施论, long-term-task-orchestration, 脚手架优于模型, agent生产落地环境重构论, 长程任务三困难]
sources: ["[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务.html"]
---
# Harness 边界移动论

Harness 边界移动论是 [[无糖可乐]] 在 [[百度Geek说]] 文章《Harness Engineering: 让 Coding Agent 可靠完成长程任务》结语（09 节）提出的方法论演化观：Harness 的每个环节都隐含着"当前模型做不到"的假设，因此会随模型能力提升而逐个过期；模型每次进化，Harness 的边界就移动一次。

## 核心主张

1. **环节即补偿**：Harness 中的程序化校验、状态持久化、脚本调度等工程化环节，本质上都是对当前模型局限（上下文遗忘、中断脆弱、行为不可控，见 [[长程任务三困难]]）的补偿机制。
2. **边界随模型移动**：模型能力提升后，原本必须由框架承担的环节可以逐步交还给模型，相应环节即告过期。
3. **判断本身不消失**：虽然具体环节会过期，但"确定哪些环节交给模型、哪些留在框架"这一判断不会消失，只是对象与结论持续变化。因此 Harness 工程是持续裁剪的动态过程，而非一次性建设。

## 操作方法论

作者给出的校准方法：每当新模型出现，从 Harness 中去掉一个环节并观察影响，以此动态确定当前模型下框架的最小必要范围。

## 与相关方法论的关系

- 与 [[脚手架优于模型]]（zhiyuanfu）互补：后者论证当前时点上脚手架的投入产出性价比，前者补充时效性维度，主张对脚手架持续裁剪而非一劳永逸。
- 与 [[agent生产落地环境重构论]]（vivo 丁俊杰）形成对照：环境重构论强调业务执行环境改造不随模型提升自动完成；边界移动论聚焦编码场景中框架层随模型能力的收缩节奏。
- 该论点也影响 [[long-term-task-orchestration]] 等 Skill 模板的长期适用性：模板中哪些环节会被模型进化吸收，同样服从边界移动逻辑。

来源：[[sources/[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务|[202604081800]HarnessEngineering让CodingAgent可靠完成长程任务]]（无糖可乐，百度Geek说，2026-04-08）。
