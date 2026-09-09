# Agent 节点体系标准（18 个）

本文档定义 Agent 节点体系的官方标准，作为**后续所有 benchmark 节点映射的唯一标准**。

## 一、一级节点（Worker Type / 分类边界）

共 4 个一级节点，用于划分能力边界：

| 一级节点 | 分类边界（职责范围） |
|---------|---------------------|
| REASONING | 推理类节点：认知处理，输入/输出均为通用 `Message` 结构，不携带业务状态 |
| CONTEXT | 上下文管理类节点：统一返回 `Context` 结构，负责当前工作空间的显式信息管理 |
| MEMORY | 长期记忆管理类节点：操作 `MemoryItem` / `MemoryDraft` / `MemoryReference`，处理持久化存储与生命周期 |
| INTERACTION | 交互与执行类节点：统一返回 `Message`，负责与外部环境、工具、用户或其他系统的通信和动作执行 |

## 二、二级节点（Node Type / 最小 API 单元）

共 18 个二级节点，按一级节点分组，使用全限定名称。

### REASONING（推理）

输入/输出均为通用 `Message` 结构，不携带业务状态，用于认知处理。

| 节点 | 输入 | 输出 | 功能 |
|------|------|------|------|
| REASONING.INFER | `messages: readonly Message[]` | `Message` | 从多轮消息中推导结论，生成初步假设、故障定位、操作意图或最终答案。 |
| REASONING.DELIBERATE | `messages: readonly Message[]` | `Message` | 对多个候选方案进行权衡比较，评估兼容性、影响范围、副作用、风险或合规性，输出经过深思熟虑的决策。 |
| REASONING.REFLECT | `message: Message; context?: readonly Message[]` | `Message` | 对照原有假设或上下文，对执行结果进行反思分析，检查错误原因、时间有效性、冲突或验证是否满足任务要求。 |
| REASONING.SAMPLE | `messages: readonly Message[]; count: number` | `readonly Message[]` | 基于输入生成多个变体或候选路径（如修复方案、检索关键词组合、答案构造），用于后续筛选或并行探索。 |

### CONTEXT（上下文）

统一返回 `Context` 结构（含 `items` 列表），负责当前工作空间的显式信息管理。

| 节点 | 输入 | 输出 | 功能 |
|------|------|------|------|
| CONTEXT.LOAD | `sources: readonly ContextSource[]`（`Message` 或 `Reference`） | `Context` | 从外部源（Issue 描述、仓库树、文件、历史轨迹等）加载初始上下文，建立工作起点。 |
| CONTEXT.SELECT | `context: Context; query: Message` | `Context` | 根据查询条件（如堆栈符号、模块名、证据优先级）筛选当前上下文中相关的条目，排除噪声或过时信息。 |
| CONTEXT.UPDATE | `context: Context; items: readonly ContextItem[]` | `Context` | 将新发现的信息（调用关系、订单状态、代码分析结果）合并到现有上下文中，扩充或修正工作知识。 |
| CONTEXT.COMPRESS | `context: Context` | `Context` | 对冗长的上下文（如日志、网页内容、代码片段）进行摘要压缩，保留核心断言、关键变量、失败断言等，减少后续计算负载。 |
| CONTEXT.RESET | `context: Context` | `Context` | 清空或重置当前上下文，用于任务切换或重新开始，返回空（或初始）上下文。 |

### MEMORY（记忆）

操作 `MemoryItem`、`MemoryDraft`、`MemoryReference`，处理持久化存储与生命周期。

| 节点 | 输入 | 输出 | 功能 |
|------|------|------|------|
| MEMORY.RETRIEVE | `query: Message` | `readonly MemoryItem[]` | 根据查询检索长期记忆中的历史事实、经验、修复模式、客户信息等，返回相关记忆条目。 |
| MEMORY.WRITE | `memories: readonly MemoryDraft[]` | `readonly MemoryItem[]` | 将新的事实、经验、来源清单或审计记录写入长期记忆，生成带标识的记忆项。 |
| MEMORY.UPDATE | `memories: readonly MemoryItem[]` | `readonly MemoryItem[]` | 更新已有记忆的内容（如状态覆盖、schema 版本变更、客户信息修正），保留时间和来源指针。 |
| MEMORY.CONSOLIDATE | `memories: readonly MemoryItem[]` | `readonly MemoryItem[]` | 合并重复或分散的记忆（如多次失败经验合并为一条仓库级经验、多轮对话整理为事务摘要），减少冗余。 |
| MEMORY.EVICT | `memories: readonly MemoryReference[]` | `readonly MemoryReference[]` | 根据引用淘汰低价值、过期、被覆盖或明确要求遗忘的记忆项，返回被淘汰的引用列表。 |

### INTERACTION（交互）

统一返回 `Message`，负责与外部环境、工具、用户或其他系统的通信和动作执行。

| 节点 | 输入 | 输出 | 功能 |
|------|------|------|------|
| INTERACTION.ACT | `action: Action`（含 `name` 和 `arguments`） | `Message` | 执行具体的操作（如代码编辑、沙箱数据库更改、MCP 工具调用、计算器运算），将操作结果封装为消息。 |
| INTERACTION.OBSERVE | `observation: Observation`（含 `source` 和 `message`） | `Message` | 接收外部观察（如测试失败堆栈、网页内容、数据库状态、API 响应），将其转换为结构化消息供后续使用。 |
| INTERACTION.COMMUNICATE | `message: Message; recipients: readonly Recipient[]` | `Message` | 向指定接收者（用户、模拟器、其他 Agent）发送消息，用于补问信息、展示费用后果、请求确认或提交最终报告。 |
| INTERACTION.OUTPUT | `message: Message` | `Message` | 将最终结果（patch、答案、事务 ID、完成状态）格式化输出，作为任务交付物或终止信号。 |

## 三、固定规则

1. 后续所有 benchmark 的节点映射**必须**以这 18 个节点为标准，不得自行增加新的节点名称。
2. 如果发现某种真实业务行为无法被现有 18 个节点准确表达，只记录为「潜在缺失节点」，**不要直接修改节点体系**。
3. 节点接口规范已经固定了节点名、输入输出以及抽象类，当前任务重点是验证覆盖情况，不是重新设计接口。

## 四、补充说明

- 案例中出现的 `CONTEXT.PROMPT` 和 `CONTEXT.SCHEDULE` 不在接口规范内，属于业务层扩展，不纳入功能定义。
- 案例中的 `INTERACTION.TOOL` 和 `INTERACTION.MCP` 为 `ACT` 的具体实现；`TERMINAL.OUTPUT` 对应 `OUTPUT`。
- 所有节点功能严格遵循输入输出契约，实现时不得改变字段名或对应关系。
