---
type: concept
title: 动态 Prompt 生成机制
tags: [prompt工程, 状态机, 意图识别, 运行时路由]
related: [aligo, 单指令状态机, workflow优先于agent, 工程确定性与ai灵活性平衡, 多智能体自进化, 上下文工程, 追加式上下文]
created: 2026-10-13
updated: 2026-10-13
sources: ["[202602140830]准确率提升至90阿里商旅基于AgentScope构建多智能体差旅助手最佳实践.html"]
---
# 动态 Prompt 生成机制

动态 Prompt 生成机制是 [[阿里商旅]] [[aligo]] 差旅助手 Prompt 工程的核心机制：不再把全部业务规则静态写入一份 Prompt，而是运行时根据对话状态动态选择/组装 Prompt 模板。文章将其本质概括为——**"为 AI 构建一个状态机：通过工程手段结合自然语言理解与程序化状态控制，精准识别用户所处的对话阶段，并将模型注意力聚焦于当前主链路，打造鲁棒性更强、更智能的对话 Agent"**。

## 反模式：线性"工作流程说明书"

把业务流程、规则、工具调用规范全量嵌入单一 Prompt，形成线性说明书：无法适配非线性动态对话，且要求模型在全量规则上实时推理用户所处状态，对模型能力要求过高。

## 实现：运行时模板选择

```python
def get_prompt_main_plan(user_input: str) -> str:
    # 运行时决策：根据规则引擎结果动态选择Prompt模板
    rule_match_result = classifier.classify(user_input)
    if rule_match_result:
        # 规则匹配成功：生成简单意图Prompt，直接路由到对应智能体
        return _get_simple_intent_prompt(.)
    else:
        # 规则匹配失败：生成复杂意图Prompt，先调用intent_recognition_agent
        return _get_complex_intent_prompt(.)
```

main_plan_agent 不是在"规则 vs 大模型"中二选一，而是把分流决策内嵌于 Prompt 组装层：规则引擎命中（快车道）生成简单意图模板直接路由，未命中（慢车道）生成复杂意图模板先调意图识别智能体。

## 定位与关联

- 与 [[单指令状态机]]（爱奇艺 Specflow）同属"运行时状态路由"思路：状态由工程维护，Prompt 随状态切换。
- 是 [[工程确定性与ai灵活性平衡]] 哲学的实现载体：状态机圈定注意力焦点与业务边界。
- 与 vivo [[追加式上下文]] 的"话题混杂"批评互证：上下文/Prompt 需按对话状态动态组织，而非线性堆叠。
- 与 [[多智能体自进化]] 中规划的"Prompt 优化智能体"关系未说明（运行时生成 vs 离线优化），待后续来源澄清。
- 状态定义、状态转移如何驱动 Prompt 片段拼装的具体实现未披露（原文截断于定义处，后续仅给抽象定义）。
