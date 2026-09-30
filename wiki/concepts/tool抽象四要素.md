---
type: concept
title: tool抽象四要素
tags: [工具系统, function-calling, schema, agent框架]
related: [薄抽象设计哲学, 子agent工具排除机制, 轻量级单进程agent框架, 结构化输出, anthropic, agent-loop]
created: 2026-09-30
updated: 2026-09-30
sources: ["[202604241629]800行代码实现OpenClaw的Tool消息总线子Agent管理架构.html"]
---
# tool抽象四要素

tool抽象四要素指将一个 LLM 工具抽象为四个成员：`name`（名称）+ `description`（描述）+ `input_schema`（参数 schema）+ `execute`（执行函数），并由 `toSchema()` 方法无中间层地转换为模型 API 的 function calling 格式。该抽象是苏雄 800 行框架的工具系统基石，与 [[结构化输出]] 所依赖的 Function Calling 机制同源。

## 实现

```typescript
export abstract class Tool {
  abstract readonly name: string;
  abstract readonly description: string;
  abstract readonly input_schema: AnthropicTool["input_schema"];
  abstract execute(args: Record<string, unknown>): Promise<unknown>;
  toSchema(): AnthropicTool {
    return {
      name: this.name,
      description: this.description,
      input_schema: this.input_schema,
    };
  }
}
```

`ToolRegistry.getToolDefinition()` 将每个工具经 `toSchema()` 输出为 `AnthropicTool[]`，无中间转换层。

## 设计要点

1. **SDK 类型即唯一 schema 定义**：`input_schema` 类型直接复用 `@anthropic-ai/sdk` 的 `Tool` 类型定义，零依赖、直接对齐模型 API。
2. **无运行时参数校验的取舍**：schema 用运行时普通对象而非 Zod / JSON Schema 库；代价是缺乏运行时校验，「LLM 传入了格式错误的参数，错误只能在执行阶段暴露」；作者判断当前规模可接受（见 [[有意取舍边界声明]]）。
3. **校验省略于入参、约束强化于工具内部**：与 EditFileTool 唯一匹配强制形成对照——入参不做类型校验，但工具内部逻辑内建语义防线（0 次匹配报错、>1 次拒绝写入，防止 LLM 模糊替换目标导致意外修改多处代码）。
4. **扩展协议**：扩展新工具只需继承 `Tool` 并注册（[[薄抽象设计哲学]] 的零侵入扩展路径之一）。
