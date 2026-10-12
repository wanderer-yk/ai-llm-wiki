---
type: source
title: "AI Agent系列｜深入解析Function Calling、MCP和Skills的本质差异与最佳实践"
tags: [function-calling, mcp, skills, agent, func-agent, 协议, 工具调用]
related: [沈询, lynxe, 千问AI平台, mcp, 一切都是函数, function-calling桥梁论, mcp薄协议转换层, 任务流程定义困难问题, skills-sub-agent包装论, skills四阶段运行流程, mcp与skills竞争关系论, skills两大未解问题, function-calling-mcp-skills本质差异对比, skills与mcp是互补还是竞争, 三大武器库, 结构化输出, function-calling大地基论, spring-ai-alibaba-studio, ai工程量化效果声明追踪]
created: 2026-10-12
updated: 2026-10-12
authors: [沈询]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247558457&idx=1&sn=088df2d9371481bb54961b09688c6796&chksm=e8a1a079b040c50564a6b6e8385ab2db4dec3db4252ddbdf64cc8649aee868a0de14b5d8855e#rd"
venue: 微信公众号「千问AI平台」
sources: ["[202602260830]AIAgent系列深入解析FunctionCallingMCP和Skills的本质差异与最佳实践.html"]
---
# AI Agent系列｜深入解析Function Calling、MCP和Skills的本质差异与最佳实践

[[沈询]]发表于微信公众号「[[千问AI平台]]」的 AI Agent 系列第三篇（2026-02-26 08:30，IP 属地浙江，标记原创，含"阿里妹导读"编辑导语），是本 Wiki 中首篇系统辨析 Function Calling、MCP 与 Skills 三者关系的来源。文章核心主张与流行"互补说"相左：**MCP 与 Skills 竞争大于互补**——两者解决同一问题（整合多既有系统完成复杂多步任务），采用不同方法（协议转换 Server + 代码型流程 vs 直接命令行 + 文字化流程），选此失彼。文章以作者自研的 [[lynxe|Lynxe（菱科斯，原 JManus）]] Func-Agent 框架为实践载体，以"一切都是函数，函数才是第一公民"的设计哲学收尾。

## 元信息与系列背景

- **作者**：沈询（`meta author`、`js_author_name` 正文区双重确认）
- **发布**：千问AI平台公众号，2026-02-26 08:30
- **系列规划（共 5 篇）**：①什么是 ReAct Agent？②Agent vs 传统编程 vs Workflow ③本篇（Function Calling/MCP/Skills）④上下文管理的一些实践 ⑤并行执行的最佳实践与我走过的弯路
- **实践载体**：[[lynxe|Lynxe]]，GitHub 地址 `https://github.com/spring-ai-alibaba/Lynxe`，作者自述"能解决我自己面对的 80% 以上的问题"
- 系列第二篇"Agent 将决策权完全下放给模型和 Prompt"的核心观点在本文被两次显式引用

## 一句话总结（三者定位）

- **Function Calling**：AI Agent 调用工具的基础能力，也是后两者能够存在的基础
- **MCP**：由 Anthropic 推动的开放标准，为 LLM 应用提供标准化接口以连接外部数据源和工具，**现已捐赠给 Linux 基金会**
- **Skills**：Anthropic Claude 的新尝试，允许用户用文字更细致地定义指令、脚本和资源；与 MCP 是竞合关系，作者明确主张"竞争关系更大一些"

## 核心论证链

1. **工具调用四步流程闭环**：①LLM 接收用户请求和工具描述 → ②LLM 决定是否调用工具并生成结构化 JSON → ③系统解析并执行工具调用 → ④结果返回 LLM，LLM 据此决定继续调用工具或生成最终回答
2. **FC 本质 = 桥梁**（[[function-calling桥梁论]]）：LLM 擅长非结构化文本，数据库/API/文件系统只能处理结构化信息，FC 在两者之间架桥，是 MCP 与 Skills 存在的前置条件
3. **MCP = 薄协议转换层**（[[mcp薄协议转换层]]）：FC + JSON-RPC 转换 + description 打包发布，价值在标准化接驳协议而非唯一接驳方式
4. **共有两难**（[[任务流程定义困难问题]]）：模型自主调度不精确 vs 全代码化丧失动态调整优势
5. **new_branch 七步 markdown 流程实证**：文字化流程实际运行"出错概率很小"（自报无口径）
6. **Skills 四阶段运行全程依赖 FC**（[[skills四阶段运行流程]]）
7. **竞争论证完整兑现**（[[mcp与skills竞争关系论]]）：同一问题两种解法，选此失彼
8. **Skills 两大未解问题**（[[skills两大未解问题]]）→ **Func-Agent 哲学收尾**（[[一切都是函数]]）

## 结构化数据（原样保留）

### Function Calling 结构化响应示例（LLM 返回）

```json
{
  "id": "chatcmpl-abc123",
  "object": "chat.completion",
  "choices": [
    {
      "index": 0,
      "message": {
        "role": "assistant",
        "content": null,
        "tool_calls": [
          {
            "id": "call_abc123",
            "type": "function",
            "function": {
              "name": "get_weather",
              "arguments": "{\"city\": \"北京\", \"date\": \"today\"}"
            }
          }
        ]
      },
      "finish_reason": "tool_calls"
    }
  ]
}
```

关键字段：`tool_calls`（工具调用信息）、`function.name`（工具名称）、`function.arguments`（JSON 字符串格式参数）、`finish_reason: "tool_calls"`。

### 系统侧解析与执行示例（JavaScript）

```javascript
// 1. 从LLM响应中提取工具调用信息
const toolCall = response.choices[0].message.tool_calls[0];
const functionName = toolCall.function.name;  // "get_weather"
const functionArgs = JSON.parse(toolCall.function.arguments);  // {city: "北京", date: "today"}

// 2. 根据工具名称找到对应的函数
const tools = {
  get_weather: (city, date) => {
    // 执行天气查询逻辑
    return `北京今天天气：25°C，晴天`;
  },
  // . 其他工具
};

// 3. 执行工具调用
const result = tools[functionName](functionArgs.city, functionArgs.date);
// 实际调用：tools["get_weather"]("北京", "today")
```

### MCP 调用链路

```javascript
LLM -> Function Calling -> MCP Client -> JSON+HTTP请求 -> MCP Server -> 既有系统（GitHub/Slack/数据库等）
                                                                                    ↓
LLM <- Function Calling结果 <- MCP Client <- JSON响应 <- MCP Server <- 既有系统返回结果
```

### Lynxe 实际运行的 new_branch 流程定义（markdown 文字化七步）

```ruby
1) 确认本地的 VERSION 与 pom.xml 与 本地branch 中的版本一致，不一致的话以pom.xml为准
2) mvn package 
3) 进入 ui-vue3 运行pnpm lint 
4) 退回项目目录， git merge upstream/main
5) 项目目录，运行 make ui-deploy
6) git 提交 branch到origin
7) git 打包 tag名字与pom的版本号一致，先删除远程tag（如果存在）：git push upstream :refs/tags/v{版本号}，然后上传tag到 upstream (上传之前请先用git remote 看一下upstream是哪里，确认是spring-ai-alibaba/JManus)
```

### Skills 运行流程

```javascript
Claude 判断是否需调用某 Skill（基于请求内容匹配已加载的 skill_name 与 description）
↓
若需要，则通过 Function Calling 调用 load_skill(skill_name)
↓
将对应 SKILL.md 的内容注入当前上下文，作为执行指令依据
↓
Claude 依照 SKILL.md 中定义的流程执行任务
↓
在执行过程中，按需通过 Function Calling：
 • 调用 bash 执行附带脚本
 • 调用 read_file 读取所需资源文件
↓
整合执行结果
```

关键标识符：`load_skill(skill_name)`、`SKILL.md`、`scripts/rotate_pdf.py`（示例脚本）、`description` 字段、JSON-RPC、约 100 token/技能元数据。

## 核心主张清单

- FC 本质 = 非结构化需求→结构化函数调用的桥梁，为 MCP/Skills 前置条件
- MCP 核心仍基于 FC，价值在标准化接驳协议（JSON-RPC + 标准化工具描述），是接驳标准而非唯一方式
- FC 与 MCP 共有"任务流程定义困难"两难
- Skills 完全依赖 FC；元数据每个约 100 token；脚本/资源经 bash/read_file 的 FC 调用
- Skills = sub-agent 包装，呼应系列第二篇"决策权下放"；代价是不可能 100% 准确
- **MCP 与 Skills 竞争大于互补**：同一问题不同解法，选此失彼
- Skills 两大未解问题：description 不结构化；仅聊天式接驳
- Lynxe 哲学："一切都是函数，函数才是第一公民"，四项函数化能力 + 双优势总结
- 自报口径声明（无量化口径）：Lynxe 解决作者 80%+ 个人问题；new_branch 流程"出错概率很小"；Skills"长流程任务成功率更高"

## 缺口与负面发现

- **标题与正文落差**：标题承诺"最佳实践"，但可提取正文中没有对应章节——文章以 Lynxe Func-Agent 哲学收尾，未出现独立的最佳实践清单
- **对比表不可提取**：文末"三者的总结性对比表"仅以图片形式存在（data-imgfileid=100074561），[[function-calling-mcp-skills本质差异对比]] 页只能基于正文论证重构
- 自报无口径主张（"80%+""出错概率很小""成功率更高"）登记 [[ai工程量化效果声明追踪]]

## 跨源关联

- **竞争论 vs 互补论**：与腾讯 CodeBuddy [[三大武器库]]（互补三层赋能）、[[skill-command-mcp三层架构]]、[[生产级skill]] 构成实质跨源张力，汇集于 [[skills与mcp是互补还是竞争]]
- **FC 地基论互证**：[[function-calling大地基论]]（觖弦/SpringAI 文）、[[结构化输出]]
- **spring-ai-alibaba 生态**：Lynxe 与 [[spring-ai-alibaba-studio]] 同组织生态
- [[mcp]] 实体页可补充本文新事实：Linux 基金会捐赠、JSON-RPC 薄转换层定位、竞争论视角

## 开放问题

- Lynxe/JManus 仓库命名关系与更名时点（`spring-ai-alibaba/Lynxe` vs upstream `spring-ai-alibaba/JManus`，全文完结仍未澄清）
- 沈询机构归属确认（阿里妹导读 + 千问AI平台 + spring-ai-alibaba 组织三重信号指向阿里巴巴，无直接证据）
- 系列第 4/5 篇（上下文管理、并行执行）发布后的追踪
