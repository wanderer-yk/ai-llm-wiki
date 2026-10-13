---
type: source
title: 从HITL(Human In The Loop) 实践出发看Agent与设计模式的对跖点
tags: [hitl, agent, 设计模式, react-agent, mcp, 人机协同]
related: [奕寰, 千问AI平台, agent设计模式对跖点, hitl通用工具设计, human-in-the-loop, react-agent, mcp, dify, mem0, spring-ai-alibaba, 工具树按需加载, 工厂模式自适应agent生成, 渐进式工具加载, workflow优先于agent]
created: 2026-10-13
updated: 2026-10-13
authors: [奕寰]
year: 2026
url: "https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247557122&idx=1&sn=1c7abefc4b557ca040dd62c45280227b"
venue: 微信公众号「千问AI平台」
sources: ["[202601080830]从HITLHumanInTheLoop实践出发看Agent与设计模式的对跖点.html"]
---
# 从HITL(Human In The Loop) 实践出发看Agent与设计模式的对跖点

本文由奕寰发表于微信公众号「[[千问AI平台]]」（2026-01-08 08:30，原创，IP 属地浙江），是本 wiki 首篇从 **Agent 基础平台工程实践**视角系统论证“Agent 机制与经典工程设计/设计模式存在一一对应关系”的来源。文章前半部分以作者在 ReactAgent 中落地 HITL 的完整实践（伪人机协同批判 → 交互设计与工具化实现 → Redis 轮询挂起的通用工具化）为引子；后半部分展开标题核心命题——八组“Agent 机制 ↔ 工程设计模式”对跖点映射，并以“把映射关系告诉 LLM、让 LLM 推理 Agent 如何进化”收束。作者结语：“Agent 目前的玩法只是冰山一角。”

## 发布信息

| 字段 | 值 |
|------|-----|
| 标题 | 从HITL(Human In The Loop) 实践出发看Agent与设计模式的对跖点 |
| 作者 | 奕寰 |
| 发布平台 | 微信公众号「千问AI平台」 |
| biz / mid / idx / sn | MzIzOTU0NTQ0MA== / 2247557122 / 1 / 1c7abefc4b557ca040dd62c45280227b |
| 发布时间 | 2026年01月08日 08:30 |
| IP 属地 | 浙江 |
| 标记 | 原创 |
| URL | `https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247557122&idx=1&sn=1c7abefc4b557ca040dd62c45280227b` |

## 一、为什么需要 HITL：两种“伪人机协同”

无通用人机协同时的两种传统做法，作者判定均为**伪人机协同**（Agent 主导完毕后用户才有参与权）：

1. **多轮对话式追问**：系统提示词定义“意图不明立即停止任务请求补充”。缺陷：本质是上下文工程中的记忆，压缩处理可能造成**语义失真**导致任务走向不可控；即便用 Assistant-user-assistant-User 循环回填原文，重新发起的对话仍需重新理解系统提示词/工具/记忆，提示词不合理或模型能力不足仍会失真。
2. **工具描述约束**：入参严格时在工具描述中要求“参数不符合则请用户补充”。缺陷：成本更高、性能更不可控——需对多个工具写不明确的参数描述；对工具按需加载的 Agent 无法保证下次提问加载到上次的工具。

作者的核心动因论断（原文加粗强调）：**多轮对话方式存在冗余——第二次对话需重新理解提示词、重新执行前置处理**。由此引出个人观点：冗余的架构设计倒推合理的代码设计，Hook/钩子/拦截这类手段的优化催生了 Human In The Loop 的人机协同体验。

HITL 实践核心两点：合理的交互设计；挂起等待保持一次对话的连续性。

## 二、HITL 实现效果与细节

- 两个演示视频 case：case1 参数不明确（Agent 挂起对话 → 用户实时补充 → 同一对话继续执行）；case2 人机协同控制任务走向。（data-mpvid: `wxv_4321075378111873038` / `wxv_4321076847040905227`）
- 交互设计：设计了支持 HTML 直接渲染的 XML 协议（对话中渲染复杂样式），用 1day 快速实现用户实时补充 UI（技术栈未披露）。
- **HITL 交互工具化**：以 functionCall 语法格式定义专门在参数补全/人类把控走向时唤起的工具；系统识别到该工具时先返回空渲染包装类（LLM 先输出工具名再输出参数），参数开始返回后输出完整交互样式；**工具出参被定义为用户补全内容**，用户问题在单次对话内完成。

需防误读（非矛盾）：作者称“用 functionCall 语法格式定义 HITL 工具”，同时自述 ReactAgent 流式解析“用的 xml 方式”——**工具定义语法与流式解析方式是两个层面**。

## 三、HITL 通用工具的工程化设计

详见 [[hitl通用工具设计]]。HITL 通用工具执行流程 7 步（原文）：

1. HITL 工具执行逻辑中，循环调用 Redis，查询 key 为 AnswerId 的数据是否有内容，没有内容时继续循环等待；
2. HITL 工具返回给 UI 界面当前对话 id 及用户需补充文案的描述（通过 url 传入 answerId 及表单参数）；
3. 用户在 UI 界面中输入；
4. UI 页面调用后端提交接口，后端接口将数据写入 Redis；
5. HITL 工具中的循环此时查询到 Redis 中的数据；
6. HITL 工具执行结束，将 Redis 中的内容作为工具的执行结果；
7. LLM 继续推理，执行下一步动作。

配套考量：

- 不能长时间空等待，需在循环中加等待时长判断（超时判断代码为截图，文本不可恢复）；
- 规划类系统提示词设计：在基础 ReactAgent 提示词中加入该工具对整体规划的影响（截图，文本不可恢复）；
- 用户隔离与对话挂起重连；
- 实现自白：用 `if (toolName.equals("HITL"))` 判断进入 HITL 工具特殊处理——“本质上这种设计就是一种拦截手段”；
- [[spring-ai-alibaba]] 的设计同样利用 Hook 钩子手段提供 HITL 能力（截图佐证，API 细节不可恢复）。

## 四、八组“Agent 机制 ↔ 工程设计模式”对跖点映射（标题核心命题）

| Agent 机制 | 对跖点（工程设计/设计模式） | 作者论据 |
|---|---|---|
| HITL | Hook、拦截（Intercept）、切面（SpringBoot AOP） | 多轮对话有冗余；工程中用钩子解耦充分利用 Context，或切面/拦截对上下文前置加工一次利用；Spring AI Alibaba 亦用 Hook 提供 HITL |
| Dify、LangGraph 流程编排 | 数据结构（图） | BPMS 类产品早已成熟流程编排；Dify 将图结构节点部分替换为 LLM；“图结构+LLM 无疑是初代 Agent 最成功的范式” |
| ReactAgent | 循环 + 责任链 | LLM 一次输入一个输出，重复思考+工具集合选择工具；循环结束条件交由 LLM 自行判断 |
| MCP | 防腐层、适配器模式 | OpenAI FunctionCall 缺标准调用协议与客户端，需 if-else 判断 RPC/HTTP 调用姿势；MCP 屏蔽调用差异 |
| 按需加载工具（Aone Copilot 工具树） | 懒加载、循环依赖（SpringBoot Bean 加载） | 首次仅加载根节点，运行时按依赖关系按需增加工具到循环调用提示词 |
| 记忆（Mem0 / Mem0-MCP / Spring AI） | 多级缓存 | Mem0 云端库+本地缓存归纳压缩；Mem0-MCP 本地内存短期记忆；Spring AI 数据库+本地内存多级缓存 |
| Skills | 简单工厂模式 | 事先利用父类原子能力创作的专有任务技能文档，每个都可看作一个工厂 |
| （展望）主 Agent 动态生成专业子 Agent | 工厂模式多态（作者称“复杂工厂模式”） | 一次对话中自适应生成解决本次任务所需的 Agent 实例，仅负责本次任务领域/规划子节点 |

完整展开见 [[agent设计模式对跖点]]；展望部分的动态子 Agent 生成提案单独见 [[工厂模式自适应agent生成]]；Aone Copilot 工具树方案见 [[工具树按需加载]]。

## 核心主张清单

1. HITL 本质动因：多轮对话存在冗余（第二次对话重新理解提示词、重新执行前置处理）。
2. 工程手段解法：钩子解耦充分利用 Context；切面/拦截对上下文前置加工一次利用。
3. `if (toolName.equals("HITL"))` 本质是拦截手段；[[spring-ai-alibaba]] 用 Hook 提供 HITL 能力。
4. 钩子/拦截/切面概念加入 Agent，催化出人机协同玩法。
5. 图结构+LLM 是初代 Agent 最成功范式；[[dify]] 类流程编排是已落地 Agent 中最可靠模式（过程/兜底可控），能力源自 BPMS 传统。
6. [[react-agent]] = 循环+责任链，终止条件交 LLM 判断；从设计模式出发 ReAct 或更早出现（反事实推测）。
7. [[mcp]] 理念契合防腐层/适配器模式；OpenAI FunctionCall 先发但缺标准调用协议与客户端，被 Anthropic 的 MCP“偷了家”（作者戏称）。
8. HITL 通用工具 7 步 Redis 轮询流程 + 超时判断 + 规划类系统提示词对整体规划影响的追加考量。
9. 记忆方案（[[mem0]] / Mem0-MCP / Spring AI 多级缓存结构）类比工程多级 Cache；Skills 类比简单工厂。
10. 展望：将设计模式↔Agent 映射告诉 LLM 让其推理 Agent 进化；工厂模式多态自适应生成专业 Agent 场景尚未出现；当前 Agent 玩法只是冰山一角。

## 张力与跨文章对照

- **Dify 定性张力**：本文称 Dify 类流程编排为“最可靠”模式（可控性视角），而 [[dify]] 页既有结论（有赞共享技术 [202601131518]）为生产环境存在性能/检索/运维瓶颈、最终走向全工程化（工程实践视角）。两者语境不同，在 [[dify]] 页并列记录、不作裁决；亦与 [[workflow优先于agent]] 的保守策略互为参照。
- **MCP 视角互补**：沈询文章簇（[[function-calling桥梁论]]、[[mcp薄协议转换层]]、function-calling-mcp-skills 本质差异对比）讨论 function-calling/MCP/Skills 差异与竞争，本文从防腐层/适配器视角为 [[mcp]] 提供正向历史叙事（OpenAI FunctionCall 被“偷家”），视角互补非冲突。
- **新范式根植经典工程底座**：与《AI工程vs传统工程道法术中的变与不变》（[202601130830]）同持这一立场，可与 [[道法术分析框架]] 关联。
- **按需工具加载两种机制**：本文的 [[工具树按需加载]]（层级树+依赖驱动，Aone Copilot 转述）与 [[渐进式工具加载]]（vivo，messages 尾部线性追加）同属一个问题域、机制不同。

## 存档与证据说明

- HTML 快照共 7 个语义 chunk：chunk 1 元数据、chunk 2–4 渠道模板 CSS、chunk 5–6 正文、chunk 7 尾部模板（weui-dialog 弹窗组件 + a11y 标签 + `</body></html>` 闭合），正文止于 chunk 6，全文定稿无遗漏。
- 截图不可恢复内容（永久缺失）：规划类系统提示词文案、超时判断代码、Spring AI Alibaba Hook API 细节、functionCall 工具定义代码。
- 评论区内容未包含在 HTML 快照中，读者补充信息无法回收。

## 开放问题

- Aone Copilot 工具树设计的一手细节（本文为二手转述，源文拼写不一 aone copilot/aone coiloit）。
- “1day”快速实现的用户补全 UI 技术栈。
- “工厂模式自适应生成 Agent”的适用场景何时出现。
- 奕寰与 [[柳遵飞（翼严）]] 是否相关（音近无证据，不可合并）。
- [[千问AI平台]] 与阿里/[[qwen]]/淘天系实体的组织关系。